###############################################################################
##
##  Copyright (C) 2014 Tavendo GmbH
##
##  Licensed under the Apache License, Version 2.0 (the "License");
##  you may not use this file except in compliance with the License.
##  You may obtain a copy of the License at
##
##      http:##www.apache.org#licenses#LICENSE-2.0
##
##  Unless required by applicable law or agreed to in writing, software
##  distributed under the License is distributed on an "AS IS" BASIS,
##  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
##  See the License for the specific language governing permissions and
##  limitations under the License.
##
###############################################################################

import os, sys, commands


## This is our default build enviroment
##
env = Environment()


## Toolchain configuration
##
if 'CXX' in os.environ:
   env["CXX"] = os.environ['CXX']

if env['CXX'].startswith('g++'):

   GCC_VERSION = commands.getoutput(env['CXX'] + ' -dumpversion')
   if GCC_VERSION < "4.3.0":
      raise SCons.Errors.UserError, "GCC version {} detected with no or insufficient C++ 11 support detected".format(GCC_VERSION)

   env.Append(CXXFLAGS = ['-std=c++11',
                          '-O2',
                          '-Wall',
                          '-pedantic',
                          '-Wno-deprecated-declarations',
                          '-Wno-unused-local-typedefs',
                          '-Wl,--no-as-needed',
			  '-DMSGPACK_USE_LEGACY_NAME_AS_FLOAT',
                          '-pthread'])

   env.Append(LINKFLAGS = ['-pthread'])

   print("Using GNU toolchain")

elif env['CXX'].startswith('clang++'):

   env.Append(CXXFLAGS = ['-std=c++11',
                          '-stdlib=libc++',
                          '-O2',
                          '-Wall',
                          '-pedantic',
                          '-Wno-unused-value',
                          '-Wno-deprecated',
			  '-DMSGPACK_USE_LEGACY_NAME_AS_FLOAT',
                          '-pthread'])

   env.Append(LINKFLAGS = ['-stdlib=libc++', '-pthread'])

   print("Using clang toolchain")

else:
   raise SCons.Errors.UserError, "Don't know how to configure build environment for toolchain {}".format(env['CXX'])


## Boost
##
if os.environ.has_key('BOOST_ROOT'):
   env.Append(CPPPATH = [os.environ['BOOST_ROOT']])
   env.Append(LIBPATH = [os.path.join(os.environ['BOOST_ROOT'], 'stage', 'lib')])
elif os.environ.has_key('BOOST_INCLUDES') and os.environ.has_key('BOOST_LIBS'):
   env.Append(CPPPATH = [os.environ['BOOST_INCLUDES']])
   env.Append(LIBPATH = [os.environ['BOOST_LIBS']])
else:
   raise SCons.Errors.UserError, "Neither BOOST_ROOT, nor BOOST_INCLUDES + BOOST_LIBS was set!"


## MsgPack
##
if os.environ.has_key('MSGPACK_ROOT'):
   env.Append(CPPPATH = [os.path.join(os.environ['MSGPACK_ROOT'], 'include')])
   env.Append(LIBPATH = [os.path.join(os.environ['MSGPACK_ROOT'], 'lib')])
elif os.environ.has_key('MSGPACK_INCLUDES') and os.environ.has_key('MSGPACK_LIBS'):
   env.Append(CPPPATH = [os.environ['MSGPACK_INCLUDES']])
   env.Append(LIBPATH = [os.environ['MSGPACK_LIBS']])
else:
   raise SCons.Errors.UserError, "Neither MSGPACK_ROOT, nor MSGPACK_INCLUDES + MSGPACK_LIBS was set!"


## Autobahn
##
env.Append(CPPPATH = ['#/autobahn'])


## Examples
##
Export('env')
examples = SConscript('examples/SConscript', variant_dir = 'build/examples', duplicate = 0)
