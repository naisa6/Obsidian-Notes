### Detecting blind OS command injection using time delays

You can use an injected command to trigger a time delay, enabling you to confirm that the command was executed based on the time that the application takes to respond. The `ping` command is a good way to do this, because lets you specify the number of ICMP packets to send. This enables you to control the time taken for the command to run:

`& ping -c 10 127.0.0.1 &`

This command causes the application to ping its loopback network adapter for 10 seconds.

• sleep command injection to every parameter

• Identify user input points that interact with the application's code execution.

• Test for code injection vulnerabilities, such as operating system (OS) command injection or remote code execution (RCE).

• Verify if proper input validation and sanitization techniques are implemented.

• Assess the effectiveness of output encoding and secure coding practices.

• Check if server-side security measures like executing code within a restricted environment are implemented.