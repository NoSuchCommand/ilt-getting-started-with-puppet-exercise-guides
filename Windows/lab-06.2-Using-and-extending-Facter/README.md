# Lab 6.2 Using and extending facter

In this lab, you will become familiar with the Facter tool by learning how to:

* Use Facter to display core fact values on a single node.
* Use Facter to display a structured fact.
* Install an external fact script and run it with Facter.
* Display fact values in the Puppet Enterprise console.

## Steps

Facter is invoked on managed nodes during each Puppet agent run. Facter gathers information about the node and sends it to the Puppet master for use during the catalog compilation phase. Typical information gathered by Facter might include IP address, amount of installed memory, kernel version, and other machine-specific information.

Run the following commands on your Windows or Linux classroom agent node, not on your local workstation.

**_Steps 1-4 can be executed from either Windows or Linux. The output will vary, but the commands are the same. Try on both operating systems if you have time to compare the differences._**

### Use Facter to see all facts about a node

Run Facter on an agent node to see a list of all facts it can determine about that node (your output will differ from this abridged sample output):

```plaintext
PS C:\Users\Administrator> facter
  aio_agent_version => 5.5.1
  augeas => {
    version => "1.10.1"
  }
  disks => {
    nvme0n1 => {
      model => "Amazon Elastic Block Store",
      size => "30.00 GiB",
      size_bytes => 32212254720
    }
  }
  dmi => {
    bios => {
      release_date => "10/16/2017",
      vendor => "Amazon EC2",
      version => "1.0"
    }
  },
  is_virtual => true
  [...]
```

### Use Facter to see a single, structured fact about a node

You can also invoke Facter so it will display a single fact instead of the complete set of facts that it knows about.

Notice in the previous sample output, the `is_virtual` fact is a simple string value while others are a nested structure of values. The latter type are called **structured facts**.

View a single, structured fact about the agent node's `os` fact (your output will differ from this abridged sample output):

```plaintext
PS C:\Users\Administrator> facter os
{
  architecture => "x64",
  family => "windows",
  hardware => "x86_64",
  name => "windows",
  release => {
    full => "10",
    major => "10"
  },
  windows => {
    edition_id => "Professional",
    installation_type => "Client",
    product_name => "Windows 10 Pro",
    release_id => "1903",
    system32 => "C:\WINDOWS\system32"
  }
}
```

### Use Facter to see a simple fact about a node

Facter also gathers and displays facts that have simple string values.

View a simple string fact about the agent node's `kernel` fact (your output will differ from this sample output if you are on Windows):

```plaintext
PS C:\Users\Administrator> facter kernel
windows
```

### View facts in the Puppet Enterprise console

So far, you have explored using Facter at the command line. As noted previously, facts gathered by Facter are sent to the Puppet master during catalog compilation. They are then available for display in the Puppet Enterprise console.

Open the console and click the **INSPECT > Nodes** submenu. Clicking a node in the list takes you to a page that displays the list of facts gathered from that node. Notice that the list displays both simple string facts and structured facts.

### Use Facter with an external script

Facter gathers built-in (core) facts that are packaged with Facter. It can also use scripts written by you or a third party to gather custom or external facts. In this exercise, you will install an external fact script that creates a `datacenter` fact. You will then verify that Facter can use the script to retrieve that fact.

First, check that the `datacenter` fact does not already exist. This command should produce no value:

```facter datacenter```

Next, move the provided external fact script into place:

1. Open Windows Explorer.
2. Download `datacenter.ps1` from this presentation's download section.
3. Move the script into place where Facter can execute it by copying `datacenter.ps1` to `C:\ProgramData\PuppetLabs\facter\facts.d`
4. View the PowerShell script in Microsoft Visual Studio Code. The script echoes a key/value pair and exits.

Finally, verify that the fact has been installed and is accessible by Facter. Notice the new output.

```plaintext
PS C:\Users\Administrator> facter datacenter
pdx
```

## Discussion questions

* Are there any unusual aspects of your company's infrastructure that you might want to capture with external fact scripts?
* Which is easier: remembering the single syntax used by Facter, or the many syntaxes used by different command line tools for collecting the same facts?

|  [Previous Lab](../lab-06.1-Puppet-resources)  |  [Next Lab](../lab-07.1-Puppet-Forge)  |
