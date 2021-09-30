# LearnIBL

## Getting Started
```bash
git clone --recursive https://github.com/mshnb/LearnIBL.git
cd Build

# UNIX Makefile
cmake ..

# Mac OSX
cmake -G "Xcode" ..

# Microsoft Windows
cmake -G "Visual Studio 16" ..
cmake -G "Visual Studio 16 Win64" ..
...
```

If you work with Visual Studio, don't forget to set LocalDebuggerWorkingDirectory to $(OutputPath), or you couldn't load models and textures correct.
