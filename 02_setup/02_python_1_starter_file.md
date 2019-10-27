Now that the virtual environment is ready, we can start cloning the project and working on it.

I'll go through what we're going to do in the section:

1. Fork the starter repo from git.
2. Clone the forked starter repo into the virtual environemnt.
3. Install the packages from the `requirements` file.


### Fork the Repo:

Go into the starter [GitHub repository](https://github.com/lailalelouch/crumble_mumble). On the right hand corner there is a `fork` button; press that. A pop-up will show up asking you which account would you like to fork this repo to, obviously you'd want to fork it to your account so choose that. What this does is it makes a copy of this repo under your username.

### Clone the Repo:

In your terminal 
```shell
(env)$ git clone https://github.com/<YOUR_USERNAME>/crumble_mumble.git
(env)$ cd crumble_mumble
```

### Install the requirements:

In every django project there is a `requirements` file that the developer creates. This text file basically has the names and versions of all the packages that need to be installed for the project to work. The convention is to either call this file `requirements.txt` or `reqs.txt`.

```
(env)$ pip install -r requirements.txt
```
