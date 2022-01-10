# Tacton Basic Logging

This is a mostly log4j1.x compatible logging framework without any networking capabilities.

This code base started as a fork of the log4j 1.2.12 code base.


## About known log4j 1.x vulnerabilities

Log4j 1.x is no longer being maintained and the known vulnerabilities were not resolved in the original project.
This project handles each of these vulnerabilities specifically. 

This is also the source of the differences to the original log4j 1.2.17 branch.

### CVE-2019-17571

Included in Log4j 1.2 is a SocketServer class that is vulnerable to deserialization of untrusted data which can be exploited to remotely execute arbitrary code when combined with a deserialization gadget when listening to untrusted network traffic for log data. This affects Log4j versions up to 1.2 up to 1.2.17.

**tacton-basic-logging resolution:**
The SocketServer class and all other classes in the org.apache.log4j.net package was completely removed.

### CVE-2020-9488

Improper validation of certificate with host mismatch in Apache Log4j SMTP appender. This could allow an SMTPS connection to be intercepted by a man-in-the-middle attack which could leak any log messages sent through that appender.

**tacton-basic-logging resolution:**
The SMTP appender class and all other classes in the org.apache.log4j.net package was completely removed.

### CVE-2021-4104

JMSAppender in Log4j 1.2 is vulnerable to deserialization of untrusted data when the attacker has write access to the Log4j configuration. The attacker can provide TopicBindingName and TopicConnectionFactoryBindingName configurations causing JMSAppender to perform JNDI requests that result in remote code execution in a similar fashion to CVE-2021-44228. Note this issue only affects Log4j 1.2 when specifically configured to use JMSAppender, which is not the default. Apache Log4j 1.2 reached end of life in August 2015. Users should upgrade to Log4j 2 as it addresses numerous other issues from the previous versions.

**tacton-basic-logging resolution:**
The JMSAppender class and all other classes in the org.apache.log4j.net package was completely removed.
