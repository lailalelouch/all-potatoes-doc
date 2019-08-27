Now that the virtual environment is ready, we can start cloning the project and working on it.

I'll go through what we're going to do in the section:

1. Fork the starter repo from git.
2. Clone the forked starter repo into the virtual environemnt.
3. Install the packages from the `requirements` file.


### Fork the Repo:

Go into the starter [GitHub repository](https://github.com/lailalelouch/all_potatoes). On the right hand corner there is a `fork` button; press that. What this does is it makes a copy of this repo under your username.

### Clone the repo:

In your terminal 
```shell
(env)$ git clone https://github.com/<YOUR_USERNAME>/all_potatoes.git
(env)$ cd all_potatoes
```

### Install the requiremnets:

In every django project there is a `requirements` file that the developer creates. This text file basically has the names and versions of all the packages that need to be installed for the project to work. The convention is to either call this file `requirements.txt` or `reqs.txt`.

```
(env)$ pip install -r requirements.txt
```
