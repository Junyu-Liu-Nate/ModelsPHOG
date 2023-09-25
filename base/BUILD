
cc_library(name = "base",
           srcs = ["base.cpp",
                   "fileutil.cpp",
                   "stringprintf.cpp",
                   "stringset.cpp",
                   "strutil.cpp",
                   "termcolor.cpp",

                   "base.h",
                   "fileutil.h",
                   "stringprintf.h",
                   "stringset.h",
                   "strutil.h",
                   "termcolor.h",

                   "nbest.h",
                   "rwlock.h",
                   "updatable_priority_queue.h",
                   "simple_histogram.h",
                   "readerutil.h",
                   "maputil.h",
                   "treeprinter.h",
                  ],
           hdrs = ["fileutil.h"],
           deps = [
                "@glog//:glog",
                "@gflags//:gflags",
           ],
           copts = [
                "-Iexternal/gflags/include",
           ],
           linkopts = ["-lgflags", "-lglog"],
           visibility = ["//visibility:public"])

