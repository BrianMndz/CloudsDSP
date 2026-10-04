cmake_minimum_required(VERSION 3.24)

project(CloudsDSP VERSION 0.0.1 LANGUAGES CXX)

add_library(clouds_dsp STATIC
    clouds/resources.cc
    clouds/dsp/correlator.cc
    clouds/dsp/granular_processor.cc
    clouds/dsp/mu_law.cc
    clouds/dsp/pvoc/frame_transformation.cc
    clouds/dsp/pvoc/phase_vocoder.cc
    clouds/dsp/pvoc/stft.cc
    stmlib/utils/random.cc
    stmlib/dsp/atan.cc
    stmlib/dsp/units.cc
)

add_library(CloudsDSP::clouds_dsp ALIAS clouds_dsp)

# C++14 is enough for this code and lets consumers build with older compilers
# such as the Bela board's Clang 3.9. A public C++17 requirement would force
# every consumer, including C++14 ones, to compile as C++17.
target_compile_features(clouds_dsp PUBLIC cxx_std_14)

# Repository root: lets consumers include "clouds/..." and "stmlib/..."
target_include_directories(clouds_dsp PUBLIC
    ${CMAKE_CURRENT_LIST_DIR}
)

# TEST=1 must be PUBLIC: clouds/stmlib headers branch on it, so consumers
# compiling those headers need the same definition as this library did.
target_compile_definitions(clouds_dsp PUBLIC
    TEST=1
)