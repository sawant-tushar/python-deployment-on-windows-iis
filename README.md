# Python Deployment on Windows IIS Server
A step-by-step guide to configure and deploy a Python/FastAPI application on Windows IIS Server.

## Step 1: Install Python

1.  Download the web installer from <https://www.python.org/downloads/>.
2.  Run the installer as Administrator.
3.  Select custom installation for all users.
4.  Choose an installation directory such that there are no white spaces
    in the path.
5.  Check the box for **Add Python to PATH/environment variables**.
6.  After the installation is completed, ensure that the environment
    variables are added to the PATH.

## Step 2: Install wfastcgi

1.  Open Windows PowerShell as Administrator.
2.  Go to the project directory and run the following command to install
    all the required extensions used for the project:

``` powershell
pip install -r requirements.txt
```

### Configuring IIS

Install the Web Server Gateway Interface (WSGI).

To run FastAPI on IIS, you need to use a WSGI server. Install `wfastcgi`
using pip.

In the terminal, run:

``` powershell
pip install wfastcgi
```

Then enable it.

## Step 3: Configure Website/Application in IIS

1.  Open **IIS Manager**: Launch IIS Manager on your server.
2.  Add a new site:
    -   Right-click on **Sites**.
    -   Select **Add Website**.
    -   Fill in the site name.
    -   Enter the physical path to your FastAPI/Python application.
    -   Enter the port number.

## Step 4: Configure IIS Application Settings

1.  Click on the server's name on the left-side panel in IIS.
2.  On the right-hand side, go to **Application Settings**.
3.  Add the following two entries:
    -   `PYTHONPATH`
    -   `WSGI_HANDLER`

## Step 5: Configure Fast CGI Settings

In the site settings, go to **Handler Mappings** and add a new module
mapping.

Configure the following:

-   **Request path:** `*`
-   **Module:** `FastCgiModule`
-   **Executable:** Path to your Python executable, for example:
    `C:\Python312\python.exe`
-   **Name:** `Python Fast API`

### Set Environment Variables

In the Fast CGI settings, set the environment variable `WSGI_HANDLER` to
your application's entry point, for example:

``` text
main:app
```

After adding the above setting, your application's `web.config` will
look something like this.

## Step 6: Configure Application Pool Settings

1.  Create a new Application Pool:
    -   Right-click on **Application Pools**.
    -   Select **Add Application Pool**.
    -   Set the **.NET CLR version** to **No Managed Code**.
2.  Assign the Application Pool:
    -   Assign the newly created application pool to your FastAPI/Python
        site.
3.  You might have to restart the website after configuration changes.
    -   The option will be under **Actions** on the right.
4.  You can browse your URL in the browser.

