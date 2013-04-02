SMART Medications Sample
========================

This is a simple sample App to demonstrate basic usage of SMART's [iOS framework][framework]. To get the sample app running check it out via git:

    $ git clone git://github.com/chb/SMARTMedSample-ios.git
	$ cd SMARTMedSample-ios
	$ git submodule update --init --recursive

Open `SMARTMedSample-ios.workspace` in Xcode.

[framework]: https://github.com/chb/SMARTFramework-ios


Registering the App with your SMART container
---------------------------------------------

The app is pre-configured to run against our sandbox, located at `http://sandbox-api-v06.smartplatforms.org`, but you still need to create the required
configuration file. Expand the **SMARTFramework** group and right-click the file `Config-default.h`, select **show in Finder**. Duplicate this file and rename
to `Config.h`. Then make sure you have the **SMART Medications Sample** target selected and hit Run.

> **Note:** The first time the framework is built it will download and compile the [Redland] C libraries for you. This may take some time and Xcode will not
> show any progress and stay at _Running 1 of 1 custom shell scripts_ for some minutes, depending on your machine. Just be patient.

> **Note:** Xcode, after building the Redland C libraries for the first time, may not find the redland headers. Simply close the project and re-open it again.

[redland]: http://librdf.org


What is happening?
------------------

The app delegate holds a reference to the `SMServer` instance `smart`. Upon App launch, the app delegate sets up this instance with:

	self.smart = [SMServer serverWithDelegate:self];

This handle to the server serves as basis for all SMART related actions. You use it to receive a `SMRecord` instance, which represents the currently active
record and which offers methods to receive medical data from the SMART container.


### Selecting a record and getting data ###

Look at the method `selectRecord:` in `MedListViewController.m`. This method is being called when the user taps the button top left. It calls the method
`selectRecord:` on our app delegate's `smart` object. Like all server methods in the framework, this method receives a callback once the operation completed.

If record selection was successful, we immediately use the `activeRecord` property to fetch all medications for this record. Medications come back in yet
another callback and if that was successful as well, we simply assign the returned medications array to our `meds` ivar and reload the table. All done.

