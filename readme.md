# Tacton Basic Logging

This is a mostly log4j1.x compatible logging framework without any networking capabilities.

This code base started as a fork of the log4j 1.2.17 code base, but is not vulnerable to any of the security issues currently known with log4j.

## Why this fork?

Many of Tacton's older Java projects were using log4j 1.x for many years. When log4j 1.x reached end-of-life in 2015 we didn't see any compelling reason to migrate to log4j 2.x since our logging needs for these projects were usually very basic. With the recently discovered [Log4Shell vulnerability](https://en.wikipedia.org/wiki/Log4Shell) we realize that if we want to keep using log4j 1.x for our older projects we better make sure to maintain our own fork.

Log4j 1.x was never vulnerable to the Log4Shell JNDI lookups unless you configured the logging to allow for it. This fork deletes all networking code from the original log4j code base and fixes additional security issues (like the XXE vulnerability in XML parsing).

This logging library almost 100% compatible with log4j:

* The same class names and methods are used for regular usage (logging).
* Most configuration settings are exactly identical.
* For most projects using log4j 1.2.17, this is a drop in replacement jar.

But in some aspects it is not compatible at all:

* This logging library does not do any networking.
* This logging library does not do database inserts.
* Any configuration of appenders that use networking will not work.
* The MDC classes works in Java 9 and later.


## About known log4j 1.x vulnerabilities

Log4j 1.x is no longer being maintained and the known vulnerabilities were not resolved in the original project.
This project handles each of these vulnerabilities specifically. 

The vulnerabilities in log4j are typically related to features around networking, databases and JMS. These are all features that were completely removed from this library, since we only want/have to support very basic use cases.

This is also the source of the differences to the original log4j 1.2.17 branch.

### CVE-2019-17571

Included in Log4j 1.2 is a SocketServer class that is vulnerable to deserialization of untrusted data which can be exploited to remotely execute arbitrary code when combined with a deserialization gadget when listening to untrusted network traffic for log data. This affects Log4j versions up to 1.2 up to 1.2.17.

**tacton-basic-logging resolution:**
The SocketServer class and all other classes in the org.apache.log4j.net package were completely removed.

### CVE-2020-9488

Improper validation of certificate with host mismatch in Apache Log4j SMTP appender. This could allow an SMTPS connection to be intercepted by a man-in-the-middle attack which could leak any log messages sent through that appender.

**tacton-basic-logging resolution:**
The SMTP appender class and all other classes in the org.apache.log4j.net package were completely removed.

### CVE-2021-4104

JMSAppender in Log4j 1.2 is vulnerable to deserialization of untrusted data when the attacker has write access to the Log4j configuration. The attacker can provide TopicBindingName and TopicConnectionFactoryBindingName configurations causing JMSAppender to perform JNDI requests that result in remote code execution in a similar fashion to CVE-2021-44228. Note this issue only affects Log4j 1.2 when specifically configured to use JMSAppender, which is not the default. Apache Log4j 1.2 reached end of life in August 2015. Users should upgrade to Log4j 2 as it addresses numerous other issues from the previous versions.

**tacton-basic-logging resolution:**
The JMSAppender class and all other classes in the org.apache.log4j.net package were completely removed.

### CVE-2022-23302

JMSSink in all versions of Log4j 1.x is vulnerable to deserialization of untrusted data when the attacker has write access to the Log4j configuration or if the configuration references an LDAP service the attacker has access to. The attacker can provide a TopicConnectionFactoryBindingName configuration causing JMSSink to perform JNDI requests that result in remote code execution in a similar fashion to CVE-2021-4104. Note this issue only affects Log4j 1.x when specifically configured to use JMSSink, which is not the default. Apache Log4j 1.2 reached end of life in August 2015. Users should upgrade to Log4j 2 as it addresses numerous other issues from the previous versions.

**tacton-basic-logging resolution:**
The JMSSink class and all other classes in the org.apache.log4j.net package were completely removed.

### CVE-2022-23305

By design, the JDBCAppender in Log4j 1.2.x accepts an SQL statement as a configuration parameter where the values to be inserted are converters from PatternLayout. The message converter, %m, is likely to always be included. This allows attackers to manipulate the SQL by entering crafted strings into input fields or headers of an application that are logged allowing unintended SQL queries to be executed. Note this issue only affects Log4j 1.x when specifically configured to use the JDBCAppender, which is not the default. Beginning in version 2.0-beta8, the JDBCAppender was re-introduced with proper support for parameterized SQL queries and further customization over the columns written to in logs. Apache Log4j 1.2 reached end of life in August 2015. Users should upgrade to Log4j 2 as it addresses numerous other issues from the previous versions.

**tacton-basic-logging resolution:**
The JDBCAppender class and all other classes in the org.apache.log4j.jdbc package were completely removed.

### CVE-2020-9493

A deserialization flaw was found in Apache Chainsaw versions prior to 2.1.0 which could lead to malicious code execution.

**tacton-basic-logging resolution:**
The Apache Chainsaw classes were completely removed.


## How about documentation?

We refer to the [log4j 1.2 documentation](https://logging.apache.org/log4j/1.2/manual.html) and we currently have no intention of publishing documenation for this version. Anything that uses networking has been removed. If you need networking in your logging library we recommend using something else.
