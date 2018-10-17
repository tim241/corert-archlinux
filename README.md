# CoreRT on Arch Linux

# Note
only the C++ code generator works on Arch Linux

# Prerequisites
```bash
sudo pacman -S dotnet-host \
                dotnet-runtime \
                dotnet-sdk \
                clang llvm cmake \
                git base-devel \
                python \
                tar
```

# Installing libobjwriter.so
```bash
cd package/libobjwriter-bin
makepkg -sci
```

# Configure project to use CoreRT
```bash
curl https://raw.githubusercontent.com/tim241/corert-archlinux/master/nuget.config -o nuget.config

dotnet add package Microsoft.DotNet.ILCompiler -v 1.0.0-alpha-*
```

# Configuring CoreRT to generate C++ code
Add this to the PropertyGroup of your csproj

```
<NativeCodeGen>cpp</NativeCodeGen>
```

# Compiling
```bash
export CppCompilerAndLinker=clang
dotnet publish -r linux-x64
```

