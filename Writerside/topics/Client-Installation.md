# Client Installation

<warning>
    The game requires a Java Runtime Environment (JRE) of at least version 21 and sufficient disk space to run.
</warning>

## Quick Installation

To install the ColdCase client, follow these steps:

1. **Download the latest release:**
    - Visit [ColdCase Releases](https://github.com/under-the-oaks/ColdCase-Client/releases/latest).
    - Download the `ColdCase-1.x.x.zip` file (replace `1.x.x` with the latest version).

2. **Extract the archive:**
    - Choose a suitable directory and extract the contents of the `.zip` file.

3. **Run the game:**
    - Execute the JAR file to start the game:
      ```sh
      java -jar Cold\ Case-*.jar
      ```
      
<note>
If you want to connect to your custom server, follow the instructions in <a href="Connecting-to-a-custom-server.md">this</a> guide.
</note>

## Building the Client Manually

If you prefer to build the game from source, follow these steps:

1. **Clone the repository:**
   ```sh
   git clone https://github.com/under-the-oaks/ColdCase-Client.git
   ```

2. **(only on Linux) Make Gradle wrapper executable:**
   ```sh
   chmod +x ./gradlew
   ```

3. **Build the release version:**
   <note>
      change the command to <code>.\gradlew release</code> on Windows.
   </note>
   ```sh
   ./gradlew release
   ```
   This will generate a `.zip` file containing the built game that can be found under `lwjgl3/build/release/`

4. **Install the built version:**
    - Extract the generated `.zip` file in a suitable directory.
    - Run the game using the JAR file as described in the quick installation.

[//]: # (- Java Runtime Environment &#40;JRE&#41; **version 21 or later**.)

[//]: # (- Sufficient disk space for installation and execution.)
