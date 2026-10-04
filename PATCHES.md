# Patch Log

## Policy

- Build-only: build-system changes that do not alter compiled DSP behavior.
- Behavior-neutral: source changes intended to preserve output.
- Behavior-changing: intentional changes to DSP behavior or public semantics.

## Baseline import

Status: no modifications to imported DSP source.
Upstream commit: 08460a69...

## Explicit initialization for portable object storage

- Classification: behavior-neutral.
- Problem: the upstream global processor relied on static zero-initialization.
- Changed files: `clouds/dsp/granular_processor.cc`,
  `clouds/dsp/grain.h`, `clouds/dsp/granular_sample_player.h`, and
  `clouds/dsp/fx/reverb.h`.
- Initialized state: processor state, feedback/tail buffers, reverb decay,
  grain quality, and grain phasor.
- Verification:
    - Density 0.75 ordinary and patterned automatic storage:
      `9b9c09ee3a612eab0226b0e4214f7f10448d7d1be1b57d21f93d9566b06298e3`.
    - Density 0.25 zero-filled and patterned automatic storage:
      `57959d7f661ad53da6c3830508cecec10e3be902c00933c4f1eb9ec1d970e140`.

## C++14 public language requirement

- Classification: build-only.
- Problem: the public `cxx_std_17` requirement forced every consumer to compile
  as C++17, but the Bela board's Clang 3.9 cannot build the CloudsEngine
  portable engine as C++17. That compiler rejects the array template argument
  in `granular_processor.h` (`src_filter_1x_2_45`) in C++1z mode as a pointer
  to a subobject, while accepting it in C++14 mode.
- Change: `CMakeLists.txt` now requires `cxx_std_14`. No source file changed,
  and consumers may still compile as C++17.
- Verification:
    - All imported sources pass a Clang 3.9 `-std=c++14` syntax check against
      Debian 9 armhf headers.
    - CloudsEngine's four quality renders are byte-identical before and after
      the change; stereo/16-bit/32-kHz remains
      `9b9c09ee3a612eab0226b0e4214f7f10448d7d1be1b57d21f93d9566b06298e3`.
