# DBngin MySQL Fixer (Windows)

A batch script to fix **DBngin** MySQL issues such as:
- Status always green (*running*) even though MySQL is actually stopped
- Unable to connect because `mysql.pid` file is stuck
- **"Port 3306 is already in use"** error due to an old `mysqld.exe` process still running

With this script, you can fix the issue without restarting your laptop.

---

## ⚠️ Disclaimer

- Use this script **at your own risk**.
- Make sure the `dbngin-go.exe` path is correct before running.
- This script kills all `mysqld.exe` processes — if you are running MySQL from other apps (XAMPP, WAMP, Laragon, etc.), those will also be stopped.
- This script **does not delete any database data**, it only removes the `mysql.pid` file (MySQL process lock file) and stops any stuck MySQL process.

---

## 📜 Features
- Close DBngin (`dbngin-go.exe`)
- Kill all `mysqld.exe` processes (freeing ports 3306 & 33060)
- Delete all `mysql.pid` files in DBngin folder
- Restart DBngin

---

## 💻 Script

Save this script as `fix-dbngin.bat`:

```bat
@echo off
echo ==========================================
echo  DBngin MySQL Fixer
echo  - Kill mysqld.exe
echo  - Remove mysql.pid
echo  - Restart DBngin
echo ==========================================

REM [1/4] Close DBngin process
echo.
echo [1/4] Closing DBngin...
taskkill /IM "dbngin-go.exe" /F >nul 2>&1

REM [2/4] Kill all MySQL processes
echo [2/4] Killing all mysqld.exe...
taskkill /IM "mysqld.exe" /F >nul 2>&1

REM [3/4] Remove mysql.pid file
echo [3/4] Removing mysql.pid file...
for /r "%LocalAppData%\com.tinyapp.DBngin\Engines\mysql" %%f in (mysql.pid) do (
    if exist "%%f" (
        del /f "%%f"
        echo   Deleted: %%f
    )
)

REM [4/4] Restart DBngin
echo [4/4] Restarting DBngin...
start "" "C:\Users\<USERNAME>\AppData\Roaming\DBngin\dbngin-go.exe"

echo.
echo Done! DBngin has been restarted.
pause
````

---

## ⚙️ Adjusting the DBngin Path

You **must adjust** the path to `dbngin-go.exe`:

```bat
start "" "C:\Users\<USERNAME>\AppData\Roaming\DBngin\dbngin-go.exe"
```

Replace `<USERNAME>` with your Windows username, or replace the entire path to match your DBngin installation location.

---

## 🔍 How to Find DBngin Path

1. **If you have a DBngin shortcut** (on Desktop or Start Menu):

   * Right-click → **Open file location**
   * If a shortcut folder opens, right-click the shortcut → **Properties**
   * Check the **Target** field — that’s your `dbngin-go.exe` path

2. **If no shortcut exists**:

   * Press `Win + S` and type `dbngin-go.exe`
   * Right-click → **Open file location**
   * Copy the shown path

3. **Manual search**:

   * Check:

     ```
     C:\Users\<USERNAME>\AppData\Roaming\DBngin\
     ```

     or

     ```
     C:\Program Files\DBngin\
     ```

---

## 🚀 How to Use

1. Save `fix-dbngin.bat` somewhere easy to access
2. **Double-click** to run it
3. DBngin will automatically:

   * Close
   * Kill any stuck MySQL process
   * Remove any stuck PID file
   * Restart

---

## 📝 Notes

* This script is for **Windows** only
* If you run other MySQL instances (XAMPP, WAMP, Laragon, etc.) on the same machine, make sure ports are not conflicting
* To change DBngin’s MySQL port, open its service settings and change from `3306` to something else (e.g., `3307`)

---

## 📌 License

Free to use and modify. Hopefully this helps other DBngin users facing similar issues.
