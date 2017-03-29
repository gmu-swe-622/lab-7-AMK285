# SWE 622 Spring 2017 - Lab 6
## ZooKeeper

In this lab, you'll implement a lock server very similar to your HW3 lock server. In this directory is a `Vagrantfile` that will set up a normal VM for you (similar to HW1, equivalent to HW2, HW3), which will have Redis installed and configured to start automatically on boot of the VM. The first time that you boot up the VM, it will try to install Redis. If it fails for any reason (for instance, network connectivity issues), try to run `vagrant provision` to have it re-try.

*VM warning*: All of the VMs I’m distributing forward several ports from your local machine into the VM for ease of use/debugging (for instance, 5005 is used for remotely debugging stuck Maven test processes). This means that you can NOT run multiple VMs from this class at the same time, or you’ll get an error. If you still have your HW1 (or HW2) VM running when you try to start this one, you’ll get an error. To resolve, run ‘vagrant suspend’ in the other VM directory, and then you’ll be able to start this one.

The idea in this lab is going to be to develop an AddressBook server and client, with the following semantics:

* The address book will be stored in Redis. Clients can list, add, and edit contacts.
* A lock server will (using ZooKeeper) maintain consistency

We want to enforce the following consistency semantics:

* A client can read the contents of an address (or the entire book) without requesting any locks
* A client can read the contents of an address, request the ability to modify it, then at some later point, modify it and release its lock. Only one client at a time may read + modify an address at the same time.


Running (from inside of VM):
```
mvn package
java -jar lock-server/target/lab6-lock-server-0.0.1-SNAPSHOT.jar
java -jar addressbook-client/target/lab6-addressbook-client-0.0.1-SNAPSHOT.jar
```
