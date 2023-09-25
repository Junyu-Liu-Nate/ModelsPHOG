# Installation

## Install bazel (Google Build System)

Execute the following instructions to get Java8 installed

```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

Download bazel

```
echo "deb http://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
curl https://storage.googleapis.com/bazel-apt/doc/apt-key.pub.gpg | sudo apt-key add -
```

Install bazel

```
sudo apt-get update
sudo apt-get install bazel
```

Get latest version of Bazel.
```
sudo apt-get upgrade bazel
```

For for information: [http://www.bazel.io/docs/install.html]

## Install prerequisites

```
sudo apt-get install libgoogle-glog-dev libgflags-dev libargtable2-dev cmake
```

# Compile


```
bazel build -c opt //... --cxxopt="-fopenmp" --linkopt="-fopenmp" --cxxopt="-DGTEST_HAS_TR1_TUPLE=0"
```

In case you see an error, you may need to set java home by calling: export JAVA_HOME=/usr/lib/jvm/java-8-oracle

On mac 2023-09-24:
```
bazel build -c opt //... --cxxopt="-Xpreprocessor" --cxxopt="-fopenmp" --linkopt="-L/opt/homebrew/Cellar/libomp/17.0.1/lib" --linkopt="-lomp" --cxxopt="-DGTEST_HAS_TR1_TUPLE=0"
```
Expected output should be:
```
Junyu-Liu-MacBook-Pro-M1-Pro-5:ModelsPHOG liujunyu$ bazel build -c opt //... --cxxopt="-Xpreprocessor" --cxxopt="-fopenmp" --linkopt="-L/opt/homebrew/Cellar/libomp/17.0.1/lib" --linkopt="-lomp" --cxxopt="-DGTEST_HAS_TR1_TUPLE=0"
INFO: Build option --linkopt has changed, discarding analysis cache.
INFO: Analyzed 14 targets (0 packages loaded, 528 targets configured).
INFO: Found 14 targets...
INFO: From Linking phog/tree/tree_index_test:
ld: warning: ignoring duplicate libraries: '-lc++', '-lgflags', '-lglog'
INFO: From Linking phog/dsl/tgen_program_test:
ld: warning: ignoring duplicate libraries: '-lc++', '-lgflags', '-lglog'
INFO: From Linking phog/dsl/branched_cond_test:
ld: warning: ignoring duplicate libraries: '-lc++', '-lgflags', '-lglog'
INFO: From Linking phog/dsl/simple_cond_test:
ld: warning: ignoring duplicate libraries: '-lc++', '-lgflags', '-lglog'
INFO: From Linking phog/dsl/tcond_language_test:
ld: warning: ignoring duplicate libraries: '-lc++', '-lgflags', '-lglog'
INFO: From Linking phog/tree/pbox_test:
ld: warning: ignoring duplicate libraries: '-lc++', '-lgflags', '-lglog'
INFO: From Linking phog/model/evaluate:
ld: warning: ignoring duplicate libraries: '-lc++', '-lgflags', '-lglog', '-lm'
INFO: From Linking phog/tree/tree_test:
ld: warning: ignoring duplicate libraries: '-lc++', '-lgflags', '-lglog'
INFO: Elapsed time: 2.168s, Critical Path: 2.00s
INFO: 21 processes: 9 internal, 12 darwin-sandbox.
INFO: Build completed successfully, 21 total actions
```

# Run tests

```
bazel test -c opt //... --cxxopt="-fopenmp" --linkopt="-fopenmp" --cxxopt="-DGTEST_HAS_TR1_TUPLE=0"
```
You can also use -c dbg instead of -c opt to run with debug version of the code.

On mac 2023-09-24:
```
bazel test -c opt //... --cxxopt="-Xpreprocessor" --cxxopt="-fopenmp" --linkopt="-L/opt/homebrew/Cellar/libomp/17.0.1/lib" --linkopt="-lomp" --cxxopt="-DGTEST_HAS_TR1_TUPLE=0"
```
Expected output should be:
```
Junyu-Liu-MacBook-Pro-M1-Pro-5:ModelsPHOG liujunyu$ bazel test -c opt //... --cxxopt="-Xpreprocessor" --cxxopt="-fopenmp" --linkopt="-L/opt/homebrew/Cellar/
libomp/17.0.1/lib" --linkopt="-lomp" --cxxopt="-DGTEST_HAS_TR1_TUPLE=0"
INFO: Build options --cxxopt and --linkopt have changed, discarding analysis cache.
INFO: Analyzed 14 targets (0 packages loaded, 528 targets configured).
INFO: Found 7 targets and 7 test targets...
INFO: From Compiling json/jsoncpp.cpp:
INFO: Elapsed time: 2.638s, Critical Path: 2.50s
INFO: 25 processes: 8 internal, 17 darwin-sandbox.
INFO: Build completed successfully, 25 total actions
//phog/dsl:branched_cond_test                                            PASSED in 0.7s
//phog/dsl:simple_cond_test                                              PASSED in 0.7s
//phog/dsl:tcond_language_test                                           PASSED in 0.7s
//phog/dsl:tgen_program_test                                             PASSED in 0.7s
//phog/tree:pbox_test                                                    PASSED in 0.7s
//phog/tree:tree_index_test                                              PASSED in 0.7s
//phog/tree:tree_test                                                    PASSED in 0.7s
```

# Running on existing datasets

See running details at [http://www.srl.inf.ethz.ch/phog].

## JavaScript dataset

### Setting - Best terminal prediction
Best terminal prediction (based on the E13 synthesis procedure defined in [1] with additional prunning of the tree, this is better result than shown in [1])

Command:
```
bazel-bin/phog/model/evaluate --logtostderr \
    --training_data programs_training.json \
    --evaluation_data programs_eval.json \
    --tgen_program synthesized/js/values_best.tgen
```
Test output 2023-09024:
```
I20230924 22:47:51.518471 5434639 tgen_program.cpp:141] Loading TGen program from synthesized/js/values_best.tgen
I20230924 22:47:51.550257 5434639 evaluate.cpp:51] Loading training data...
 processed files -> 100.00% [100000/100000]I20230924 22:50:57.320819 5434639 tree.cpp:2007] Parsing done.
I20230924 22:50:57.367341 5434639 tree.cpp:2014] Remaining trees after removing trees with more than 30000 nodes: 99031
I20230924 22:50:57.367403 5434639 evaluate.cpp:54] Training data with 99031 trees loaded.
I20230924 22:50:57.367419 5434639 evaluate.cpp:56] Loading evaluation data...
 processed files -> 100.00% [50000/50000]I20230924 22:52:02.955830 5434639 tree.cpp:2007] Parsing done.
I20230924 22:52:02.963433 5434639 tree.cpp:2014] Remaining trees after removing trees with more than 30000 nodes: 49536
I20230924 22:52:02.963496 5434639 evaluate.cpp:59] Evaluation data with 49536 trees loaded.
I20230924 22:52:02.963512 5434639 evaluate.cpp:61] Training...
I20230924 22:52:02.965272 5434639 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 22:52:08.671288 5434639 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 22:52:14.432714 5434639 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 22:52:20.220870 5434639 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 22:52:25.993723 5434639 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 22:52:31.633285 5434639 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 22:52:37.249866 5434639 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 22:52:42.900843 5434639 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 22:52:48.556818 5434639 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 22:52:54.224645 5434639 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 22:52:59.882928 5434639 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 22:53:13.074223 5434639 evaluate.cpp:73] Training done.
I20230924 22:53:13.074301 5434639 evaluate.cpp:80] Evaluating error rate...
I20230924 22:55:02.301764 5434639 evaluate.cpp:88] Evaluation error rate done.
error rate = 0.1406
I20230924 22:55:02.301851 5434639 evaluate.cpp:92] Done.
```

### Setting - Best non-terminal prediction
Best non-terminal prediction (based on the ID3+ synthesis procedure defined in [1], the result is the shown in [1])

Command:
```
bazel-bin/phog/model/evaluate --logtostderr \
    --training_data programs_training.json \
    --evaluation_data programs_eval.json \
    --tgen_program synthesized/js/types_id3.tgen \
    --is_for_node_type
```
Test output 2023-09024:
```
I20230924 23:01:09.960533 5447662 tgen_program.cpp:141] Loading TGen program from synthesized/js/types_id3.tgen
I20230924 23:01:09.968529 5447662 evaluate.cpp:51] Loading training data...
 processed files -> 100.00% [100000/100000]I20230924 23:04:07.125991 5447662 tree.cpp:2007] Parsing done.
I20230924 23:04:07.183980 5447662 tree.cpp:2014] Remaining trees after removing trees with more than 30000 nodes: 99031
I20230924 23:04:07.184053 5447662 evaluate.cpp:54] Training data with 99031 trees loaded.
I20230924 23:04:07.184069 5447662 evaluate.cpp:56] Loading evaluation data...
 processed files -> 100.00% [50000/50000]I20230924 23:05:11.928732 5447662 tree.cpp:2007] Parsing done.
I20230924 23:05:11.934176 5447662 tree.cpp:2014] Remaining trees after removing trees with more than 30000 nodes: 49536
I20230924 23:05:11.934221 5447662 evaluate.cpp:59] Evaluation data with 49536 trees loaded.
I20230924 23:05:11.934237 5447662 evaluate.cpp:61] Training...
I20230924 23:05:11.934867 5447662 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:05:17.207289 5447662 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:05:22.472908 5447662 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:05:27.743044 5447662 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:05:32.929508 5447662 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:05:38.017912 5447662 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:05:43.130856 5447662 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:05:48.583089 5447662 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:05:53.622869 5447662 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:05:58.774096 5447662 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:06:03.802697 5447662 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:06:18.617870 5447662 evaluate.cpp:73] Training done.
I20230924 23:06:18.617921 5447662 evaluate.cpp:80] Evaluating error rate...
I20230924 23:08:03.651901 5447662 evaluate.cpp:88] Evaluation error rate done.
error rate = 0.1611
I20230924 23:08:03.651962 5447662 evaluate.cpp:92] Done.
```

## Python

### Setting - Best terminal prediction
Best terminal prediction (based on the E13 synthesis procedure defined in [1], this is the result shown in [1])

Command:
```
bazel-bin/phog/model/evaluate --logtostderr \
    --training_data python100k_train.json \
    --evaluation_data python50k_eval.json \
    --tgen_program synthesized/py/values_e13.tgen
```
Test output 2023-09024:
```
I20230924 23:13:24.072548 5459852 tgen_program.cpp:141] Loading TGen program from synthesized/py/values_e13.tgen
I20230924 23:13:24.164009 5459852 evaluate.cpp:51] Loading training data...
 processed files -> 100.00% [100000/100000]I20230924 23:13:53.203169 5459852 tree.cpp:2007] Parsing done.
I20230924 23:13:53.203492 5459852 tree.cpp:2014] Remaining trees after removing trees with more than 30000 nodes: 100000
I20230924 23:13:53.203547 5459852 evaluate.cpp:54] Training data with 100000 trees loaded.
I20230924 23:13:53.203562 5459852 evaluate.cpp:56] Loading evaluation data...
 processed files -> 100.00% [50000/50000]I20230924 23:14:09.305212 5459852 tree.cpp:2007] Parsing done.
I20230924 23:14:09.305305 5459852 tree.cpp:2014] Remaining trees after removing trees with more than 30000 nodes: 50000
I20230924 23:14:09.305346 5459852 evaluate.cpp:59] Evaluation data with 50000 trees loaded.
I20230924 23:14:09.305361 5459852 evaluate.cpp:61] Training...
I20230924 23:14:09.318812 5459852 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:14:16.814671 5459852 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:14:24.269498 5459852 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:14:32.208619 5459852 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:14:39.294996 5459852 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:14:44.560582 5459852 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:14:49.792695 5459852 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:14:52.017652 5459852 evaluate.cpp:73] Training done.
I20230924 23:14:52.017689 5459852 evaluate.cpp:80] Evaluating error rate...
I20230924 23:15:16.894757 5459852 evaluate.cpp:88] Evaluation error rate done.
error rate = 0.3131
I20230924 23:15:16.894809 5459852 evaluate.cpp:92] Done.
```

### Setting - Best non-terminal prediction
Best non-terminal prediction (based on the ID3+ synthesis procedure defined in [1], the result is the shown in [1])

Command:
```
bazel-bin/phog/model/evaluate --logtostderr \
    --training_data python100k_train.json \
    --evaluation_data python50k_eval.json \
    --tgen_program synthesized/py/types_id3.tgen \
    --is_for_node_type
```
Test output 2023-09024:
```
I20230924 23:16:57.361811 5463710 tgen_program.cpp:141] Loading TGen program from synthesized/py/types_id3.tgen
I20230924 23:16:57.366732 5463710 evaluate.cpp:51] Loading training data...
 processed files -> 100.00% [100000/100000]I20230924 23:17:24.497709 5463710 tree.cpp:2007] Parsing done.
I20230924 23:17:24.497876 5463710 tree.cpp:2014] Remaining trees after removing trees with more than 30000 nodes: 100000
I20230924 23:17:24.497912 5463710 evaluate.cpp:54] Training data with 100000 trees loaded.
I20230924 23:17:24.497926 5463710 evaluate.cpp:56] Loading evaluation data...
 processed files -> 100.00% [50000/50000]I20230924 23:17:37.830523 5463710 tree.cpp:2007] Parsing done.
I20230924 23:17:37.830612 5463710 tree.cpp:2014] Remaining trees after removing trees with more than 30000 nodes: 50000
I20230924 23:17:37.830641 5463710 evaluate.cpp:59] Evaluation data with 50000 trees loaded.
I20230924 23:17:37.830657 5463710 evaluate.cpp:61] Training...
I20230924 23:17:37.831076 5463710 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:17:41.728960 5463710 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:17:45.672295 5463710 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:17:49.612680 5463710 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:17:53.521680 5463710 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:17:57.436977 5463710 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:18:01.332916 5463710 evaluate.cpp:68] Training... (logged every 10000000 samples).
I20230924 23:18:03.311399 5463710 evaluate.cpp:73] Training done.
I20230924 23:18:03.311443 5463710 evaluate.cpp:80] Evaluating error rate...
I20230924 23:18:29.969274 5463710 evaluate.cpp:88] Evaluation error rate done.
error rate = 0.2418
I20230924 23:18:29.969322 5463710 evaluate.cpp:92] Done.
```