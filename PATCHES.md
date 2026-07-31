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
