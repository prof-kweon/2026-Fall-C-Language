# How to Install MinGW64 on Windows

1. **Download `mingw64.zip`.** (https://smart.wsu.ac.kr/mod/ubboard/article.php?id=1077386&bwid=223379)
   Download the `mingw64.zip` file provided by your instructor.

2. **Extract the ZIP file.**
   Right-click `mingw64.zip` and select **Extract All**.

3. **Move the folder.**
   Move the extracted `mingw64` folder to:
   `C:\mingw64`

4. **Open Environment Variables.**
   Search for **Environment Variables** in the Windows Start menu.
   Select **Edit the system environment variables**.

5. **Edit the Path variable.**
   Click **Environment Variables**.
   Find **Path** under User variables and click **Edit**.

6. **Add MinGW64 to Path.**
   Click **New** and add:
   `C:\mingw64\bin`

7. **Save the settings.**
   Click **OK** to close all windows.

8. **Open a new Command Prompt.**
   Close any old Command Prompt windows and open a new one.

9. **Check the installation.**
   Type:
   `gcc --version`

10. **Done!**
    If you see the GCC/G++ version information, MinGW64 is installed correctly.
