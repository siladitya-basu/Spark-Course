# Spark Course

## Getting Started

- Clone this repo
- Download dataset
- Set data paths
- Install Apace Spark

&nbsp;
### Clone this repo

Create an empty `Spark-Course` folder and run

```
git clone https://github.com/siladitya-basu/Spark-Course.git .
```

&nbsp;
### Download dataset

Go to Kaggle and download the [IMDB dataset](https://www.kaggle.com/datasets/kunwarakash/imdbdatasets?select=title_ratings.tsv). Unzip it in `/path/to/Spark-Course/Data/imdb`.

Alternatively use Kaggle API

```
cd /path/to/Spark-Course/Data/imdb
kaggle datasets download -d kunwarakash/imdbdatasets
```

&nbsp;
### Set data paths
These notebooks use variables sourced from `/path/to/Spark-Course/Notebooks/Includes/paths.py`.

The `paths.py` file is not included and will depend on where you cloned this repo. The path variable names are in `_paths.py` file. Copy it and save as `paths.py`, and then define your paths in `paths.py`.

Remember to put `paths.py` in your `.gitignore`!

&nbsp;
### Install Apache Spark

The following shows installation and some troubleshooting instructions for Apache Spark on Arch Linux. Instructions for other OSes are easily available online.

1. Make sure Python, Scala, and JDK are installed.

2. Install Hadoop. (I'm using `paru` but any AUR helper such as `yay` or `aura` could be used.)
    ```
    paru hadoop
    ```

3. Configure Hadoop as shown in the [Arch wiki](https://wiki.archlinux.org/title/Hadoop). See the **Configuration** section.

4. Run the following to check if there are any errors.
    ```
    hadoop version
    ```
    > I was getting the following error from `/etc/profile.d/perlbin.sh`: `append_path: commmand not found`. Copied the function `append_path` from `/etc/profile` script to `perlbin.sh`.

5. Install Apache Spark.
    ```
    paru apache-spark
    ```

6. Install `openssh`. Make sure the service is running and you can connect to port 22.

    ```
    systemctl status sshd
    ssh -p 22 <username>:localhost
    ```

    >If `sshd` is not running, start it with `systemctl start sshd`.

    You will be asked for your password. Enter it to open the connection. Type exit to close the connection.

7. Go to **Configuration** in the [Arch wiki](https://wiki.archlinux.org/title/Apache_Spark) and make necessary changes.

8. Run 
    ```
    cd /opt/apache-spark/sbin
    sudo ./start-master.sh
    ```

    > I had an error while starting master; said `hostname: command not found`. Problem resolved after installing `inetutils`.

    Open a browser and go to `localhost:8080`. Note the location of master in the line at the very top: "Spark Master at `spark://<machine-name>:<port>`".
    
    Then run
    ```
    sudo ./start-worker.sh spark://<machine-name>:<port>
    ```
    (To stop them, use the `stop-*.sh` scripts.)

9. Check if `spark-shell` (Scala), `pyspark` (Python), and `spark-sql` (SQL) run without errors (might have multiple warnings). Use `:quit`, `quit()`, or `quit;`, repectively, to exit these shells.

    > My `pyspark` and `spark-sql` shells were failing to start even though `spark-shell` could start. Checking error logs, I found that I had OpenJDK version 18 set as default. Spark can only use upto OpenJDK version 11. Changed OpenJDK version using `archlinux-java`.

    > Errors while running `spark-sql`. Ran as root.

10. (Recommended) Set up a virtual environment for Spark-Course.
    1. Install `pip`.
        ```
        paru python-pip
        ```
    2. Install `pipx` using `pip`.
        ```
        pip install pipx
        ```
    3. Install `pipenv` using `pipx`.
        ```
        pipx install pipenv
        ```
    4. Change directory to where you have cloned this repo and use `pipenv` to install `numpy`, `pandas`, `matplotlib`, `pyspark`, and `delta-spark`. This will create a virtual environment for this project and install these libraries and their dependencies for this virtual environment.
        ```
        cd path/to/Spark-Course
        pipenv install pyspark
        ```
    If you're using VS Code/Codium, make sure to switch to this virtual environment.

    > Ran into space issues while installing libraries. Turns out `/tmp` was too full. Rebooting fixed the issue.

