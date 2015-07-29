# Druid Parent POM

### Prerequisites

1. Add the `sonatype-nexus-staging` entry to the `<servers>` list in your maven
   settings file (`$HOME/.m2/settings.xml`).  Replace `username` and `password`
   with your Sonatype username and password.

  ```xml
  <!-- sonatype-nexus-staging -->
  <server>
    <id>sonatype-nexus-staging</id>
    <username>username</username>
    <password>mypassword</password>
  </server>
  ```

  We recommend encrypting your maven passwords. See the maven [password
  encryption guide](http://maven.apache.org/guides/mini/guide-encryption.html)
  for advice on how to encrypt your maven passwords.

1. [Create a Sonatype account](https://issues.sonatype.org/secure/Signup!default.jspa)
   and request to be added to the list of users with deployment permissions.

1. Create a GPG key and publish it.

  - Generate the key (`brew install gpg2` to get gpg2 on a Mac)

    ```bash
    gpg2 --gen-key
    ```

    Use the default values for type, size, expiration, and add your name and
    email.

  - List the key and note the key id

    ```bash
    gpg2 --list-keys
    ```

    The output should look similar to the following, where `ABCDEF12` is your key id.

        --------------------------------
        pub   4096R/ABCDEF12 2015-02-04
        uid       [ultimate] John Doe <johndoe@example.com>
        ...

  - Upload your key to a key-server

    Use the following command, replacing `ABCDEF12` with your own key id.

    ```bash
    # use the key id as shown by gpg2 --list-keys
    gpg2 --keyserver hkp://pool.sks-keyservers.net --send-keys ABCDEF12
    ```

### Cutting a release and publishing

```bash
# Make sure gpg-agent is running
eval $(gpg-agent --daemon)

# Prepare a release for the parent pom
mvn release:clean release:prepare

# Perform the release
mvn release:perform

# once staging is complete promote the release
cd target/checkout && mvn -Prelease nexus-staging:release
```
