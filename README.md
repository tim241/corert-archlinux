# CoreRT on Arch Linux

# Note
only the C++ code generator works on Arch Linux

# Prerequisites
```
sudo pacman -S dotnet-host \
                dotnet-runtime \
                dotnet-sdk \
                clang llvm cmake \
                git base-devel \
                python \
                tar
```

# Installing libobjwriter.so
```
cd package/libobjwriter-bin
makepkg -sci
```

# Configure project to use CoreRT
```
nuget='<add key="dotnet-core" value="https://dotnet.myget.org/F/dotnet-core/api/v3/index.json" />'
nuget2='<add key="nuget.org" value="https://api.nuget.org/v3/index.json" protocolVersion="3" />'

dotnet new nuget
sed -i "s|<clear />|<clear />\n$nuget\n$nuget2\n|g" nuget.config

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
```
dotnet publish -r linux-x64
```

