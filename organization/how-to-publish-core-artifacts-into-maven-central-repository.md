# How to publish Core artifacts into Maven Central repository

**Step 1:** Create your [Sonatype JIRA Account](https://issues.sonatype.org/secure/Signup!default.jspa)

**Step 2:** Generate GPG keys

```bash
gpg --full-gen-key
# RSA and DSA (0)
# 2048
# 0 = key does not expire
# Real name: [Name Surname]
# Email address: [name.surname@promethist.ai]
```

**Step 3:** Publish GPG public key

```bash
gpg --keyserver keys.gnupg.net --send-key [KEY]
```

