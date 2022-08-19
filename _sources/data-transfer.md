# Data transfer
Once you get a sense for the different kinds of storage locations available to you as part of the HPC cluster, your next step is to actually transfer the files you need to analyze.

You can expect your cluster will have a way for you to use the command line to transfer files, but for a lot of humanities scholars, that's less comfortable than using something with a visual interface. In this section, we'll cover various options that your research computing group may or may not provide for data transfer, to at least give you some language for what to ask for.

## Globus
[Globus](https://www.globus.org/) is a service that universities can subscribe to, that allows users to transfer files between different storage locations (e.g. scratch vs HPC storage vs your home directory) using a web-based interface. If you want to also transfer files to and from your own computer, there's a small application, [Globus Connect Personal](https://www.globus.org/globus-connect-personal), that you need to install on your computer.

First, make sure that your research computing group supports Globus-based access to the HPC cluster (for scratch and your home directory) and/or HPC storage. Searching for *your institution's name + "research computing" + Globus* should turn up something if you have access.

### Setting up Globus Connect
Next, install [Globus Connect Personal](https://www.globus.org/globus-connect-personal). Once it's installed, launch the software. A window will pop up with a *Log In* button. Click the button, and it will take you to a webpage where you can choose your institution from a drop-down menu, then log in as you usually do.

The next step is to create an *endpoint* for your computer. An *endpoint* is just a place where Globus can transfer data to or from. The HPC cluster should have an endpoint, HPC storage should have an endpoint, and there may be other endpoints within your institution (e.g. possibly run by the library for transferring data from their digitized collections) that you can get access to. The names of all endpoints are publicly visible (but **not** accessible) to everyone at your institution, so you might not want to call the endpoint for your laptop something embarrassing.

You can read the [Globus instructions for how to set up an endpoint](https://docs.globus.org/faq/globus-connect-endpoints/#how_can_i_create_an_endpoint).

Globus Connect Personal may not automatically start every time you reboot your computer. Even though you'll mostly be interacting with Globus through a browser, you do need to have Globus Connect running in order to successfully transfer files to and from your computer. If you log into Globus and try to connect to the endpoint for your computer, but Globus Connect isn't running, the webpage will prompt you to start it.

### Transferring files with Globus
If you go to [https://app.globus.org/file-manager](https://app.globus.org/file-manager), after you authenticate, you'll see an interface with two panels by default. Those are two different storage locations, and you use the two panels to transfer files from one to the other. (There's no "source" and "destination" relationship inherently between those two panels; you can go in either direction.)

![Globus file manager screen](globus_filemanager.png)

The first step is to search for a collection. In Globus jargon, "collections" and "endpoints" have different functionality for people who run servers, but we can treat them as both meaning "somewhere you transfer data to and/or from". If you click in the box labeled "Search", you'll see collections that you've recently accessed, or you can start typing and it will try to autocomplete options. When it autocompletes, it will also show the owner (by email address), which can be useful to confirm, for instance, that you're trying to connect to your computer and not one with a similar endpoint name.

![Searching for Quinn as a collection in Globus](globus_search.png)

Your research computing group will tell you how to search for their endpoints.

.. 🌲 ::
   The Globus endpoint for Sherlock is [SRCC Sherlock](https://app.globus.org/file-manager?origin_id=6881ae2e-db26-11e5-9772-22000b9da45e), from where you can access your and your group's home and scratch directories. To access Oak, you can use [SRCC Oak](https://app.globus.org/file-manager?origin_id=8b3a8b64-d4ab-4551-b37e-ca0092f769a7).

