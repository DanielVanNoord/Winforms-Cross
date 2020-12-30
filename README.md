# Winforms-Cross
A test repository for .Net 5 WinForms and Core.System.Windows.Forms. This uses .Net 5's native WinForms for Windows and the Core.System.Windows.Forms nuget package for UI on Linux. The csproj is formatted to test methods to automatically using the correct package based on the target.
Note that you can only build the .Net 5's WinForms target on Windows. 

## Native Requirements

* Windows: .Net 5 to build, nothing to run
* Linux: .Net 5 to build, libx11 and libgdiplus