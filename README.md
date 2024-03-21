# Migration

Migration is a utility that will take your static IP address mappings in OPNsense and migrate them over to the Kea DHCP server that comes with OPNsense version 24.

- **You <u>must</u> upgrade to OPNsense version 24 before using this utility**

### Here is a video tutorial if that works best for you

[<img src="img/VideoThumb.png" width="100%">](https://youtu.be/qoGY6--vEK4)

### This is a simple tool to use:

1) [Download the program](https://github.com/EasyG0ing1/Migration/releases/latest) for your operating system (they are native binaries, no need for a Java runtime environment).
    * Create a clean folder to put the program in
2) From your OPNsense interface, go to Kea DHCP / Kea DHCPv4 / Subnets
3) Define all of your subnets and their IP Pools.
    * The tool uses those newly created subnets to automatically assign your current reservations to the correct subnet.
4) Apply those changes
5) Go to System / Configuration / Backups
6) Click on Download Configuration
    * Save the file as `config.xml` in the same folder you downloaded to tool into
    * Make a copy of it just in case
7) Open a shell (cmd.exe or terminal etc.) and go into that folder
8) run `migrate`
    - You should see that the file `new_config.xml` has been created
    - If you don't then you will see a description of some problem that was found which will help you understand what needs to be fixed in your config.xml file
9) Go back into OPNsense under backups and restore this new file.
   - Make sure you UNCKECK the box that says Reboot after restore

Done!

### [Binaries](https://github.com/EasyG0ing1/Migration/releases/latest)
The binaries were compiled and tested in these operating systems

- Windows 11 (build 21996.1)
- MacOS Sonoma 14.2.1
- Ubuntu 22.0.4 LTS (Jammy Jellyfish)

No guarantees with older versions of these operating systems.

## Summary of what this utility does
* Loads config.xml into memory
* Loads existing static maps into memory from the `staticmap` xml node
* Loads the Kea subnets from the `subnet4` xml node
* Iterates through each static mapping
  * Calculates subnet using the IP address and it's subnet mask
  * Looks for a matching Kea subnet
  * Creates a new Kea DHCP static mapping using the subnet UUID from the matched subnet
  * Assigns a new random UUID to the new static map for Kea
* Converts the new mappings into xml under the node name `reservations`
* Replaces the `reservations` xml node from the original `config.xml` file
* Saves the modified xml to a new file named `new_config.xml`

Every step along the way is checked for problems and if any are found, the program tells you exactly what you need to look for to settle the problem.

### Issues
If you have any problems that you can't figure out, create an issue and I will be happy to assist.

### Contributing
Create an Issue or a Pull Request if you want to contribute to the project.

### Updates

* 2.1.1
  * Added clear and expanded error messages so that any problems that might happen should always present the user with an exact cause of the problem along with instruction of how to correct the problem

* 2.1.0
  * Removed the need to run a check before doing the migration
  * Users will get specific feedback if there are any problems which will let them know exactly what is wrong if there are any problems with the migration.

* 2.0.1
  * Minor enhancements

* 2.0.0
  * Streamlined use of XML library, eliminating unnecessary calls.
  * Program now outputs a file that can be directly imported into OPNsense

* 1.0.1
  * Added more detailed error reporting

* 1.0.0
  * Initial Release
