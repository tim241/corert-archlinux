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

dir="$(ls ~/.nuget/packages/runtime.linux-x64.microsoft.dotnet.ilcompiler/ | head -1)"
dir2="$HOME/.nuget/packages/runtime.linux-x64.microsoft.dotnet.ilcompiler/$dir/inc"
url="https://raw.githubusercontent.com/dotnet/corert/master/src/Native/Bootstrap/"

mkdir -p "$dir2"

curl $url/common.h -o "$dir2/common.h"
curl $url/CppCodeGen.h -o "$dir2/CppCodeGen.h"
```

# Configuring CoreRT to generate C++ code
Add this to the PropertyGroup of your csproj

```
<NativeCodeGen>cpp</NativeCodeGen>
```

# Compiling
```bash
dotnet publish -r linux-x64
```

