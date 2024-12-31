# Winforms-Cross
A test repository for .Net 6 WinForms and Core.System.Windows.Forms. This uses .Net 6's native WinForms for Windows and the Core.System.Windows.Forms nuget package for UI on Linux (built on System.Drawing.Common). The csproj is configured to automatically using the correct package based on the target.

You can run the cross platform version on Windows or Linux with the command "dotnet run -f net6.0" and the .Net WinForms version on Windows with "dotnet run -f net6.0-windows".

Note that you can only build the .Net 6's WinForms target on Windows. 

## Native Requirements

* Windows: .Net 6 to build, nothing to run
    * dotnet run -f net6.0-windows will launch the WinForms version. Visual Studio will default to running this target as will.
* Linux: .Net 6 to build, libx11 and libgdiplus to run
    * dotnet run -f net6.0 will launch the cross platform version.

## Project file customization

The project file has some customizations allowing it to build with the correct WinForms provider for both Windows and Linux.

1. The OutputType is WinExe, which is used for UI Applications
2. It dual targets (TargetFrameworks) net6.0-windows for WinForms and net6.0 for cross platform WinForms
3. The Choose section changes what WinForms provider to use. If you target net6.0-windows and are on Windows (you cannot build .Net 6.0 WinForms on Linux) it will use .Net 6.0's WinForms. Otherwise it will include and use the cross platform library.

~~~
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFrameworks>net6.0-windows;net6.0</TargetFrameworks>
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
  </PropertyGroup>

  <Choose>
    <When Condition="'$(TargetFramework)' == 'net6.0-windows' and '$([System.Runtime.InteropServices.RuntimeInformation]::IsOSPlatform($([System.Runtime.InteropServices.OSPlatform]::Windows)))'">
      <PropertyGroup>
        <UseWindowsForms>true</UseWindowsForms>
      </PropertyGroup>
    </When>
    <Otherwise>
      <ItemGroup>
        <PackageReference Include="Core.System.Windows.Forms" Version="1.0.0-alpha6" />
      </ItemGroup>
    </Otherwise>
  </Choose>
</Project>
~~~

## Other notes
* Prior to version 16.9 Visual Studio had a bug that prevented it from parsing dependencies for some dual targeted csproj files. If you get the warning mark you can still build and run the project but you cannot see the dependencies. Updating to 16.9.0+ should fix this issue.