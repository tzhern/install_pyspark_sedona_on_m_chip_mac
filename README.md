# Install PySpark with Apache Sedona on M Chip Mac

This guide will help you install Apache Sedona (version 1.5.1) with PySpark (version 3.5.0) and PyArrow (version 15.0.0) on your M Chip Mac. Please follow the steps below in your command line interface (CLI), not in a Jupyter Notebook, for a smoother installation process and more informative error messages.

## Prerequisites

Before installing PySpark and Apache Sedona, ensure you have Java installed, as Spark runs on the Java Virtual Machine (JVM). Scala, the programming language for Spark, will also be needed. If you're using Homebrew, you can easily install these dependencies:

```bash
brew install java scala apache-spark
```

## Installing PySpark and Apache Sedona

### PySpark and PyArrow

Follow the installation guide from the official PySpark documentation: [Getting Started with PySpark](https://spark.apache.org/docs/latest/api/python/getting_started/install.html).

Run the following command to install PySpark and PyArrow:

```bash
pip install pyspark==3.5.0 pyarrow==15.0.0 apache-sedona==1.5.1
```

You can choose to install PySpark either in the base/root environment or within a specific Conda environment. Be aware that the most common issue during installation is PySpark not finding Java or finding an incompatible Java version.

### Verifying the Installation

To verify that PySpark is installed correctly, open a new terminal window and run:

```bash
pyspark
```

If PySpark starts without issues, proceed to test Apache Sedona in a Jupyter Notebook:

```python
# Import SedonaContext from sedona.spark
from sedona.spark import SedonaContext

# Initialize a SedonaContext with specific configurations
config = SedonaContext.builder()\
    .config('spark.jars.packages',
            # Add Sedona and GeoTools libraries as dependencies
            'org.apache.sedona:sedona-spark-3.0_2.12:1.5.1,'
            'org.datasyslab:geotools-wrapper:1.5.1-28.2')\
    .config("spark.sql.repl.eagerEval.enabled", True)  # Enable eager evaluation for Spark SQL
    .config("spark.sql.execution.arrow.pyspark.enabled", True)  # Enable Arrow-based columnar data transfers (make it looks nicer!)
    # Add an external repository for additional Spark JARs
    .config('spark.jars.repositories', 'https://artifacts.unidata.ucar.edu/repository/unidata-all')\
    .getOrCreate()  # Create or get the existing Spark Context

# Create a SedonaContext based on the configured Spark Context
sedona = SedonaContext.create(config)
```

## Troubleshooting

Installation issues often stem from incorrect path configurations. Since the exact location of software varies significantly based on hardware, OS, and the method of installation, here are some general and M chip-specific tips:

### General Advice

- **Backup Paths**: Always backup your current path and Conda environment before making changes.
- **Understand Commands**: Have a clear understanding of the purpose behind each command you run. Avoid blindly copying and pasting from the internet.
- **Refresh Terminal State**: Open new terminal windows frequently to ensure your environment variables and path are up-to-date.
- **Take Breaks**: Step away from your computer regularly to clear your mind.

### M Chip-Specific Advice

Homebrew might install a version of Java that's too recent and incompatible with your setup. Consider installing an older version of Java:

```bash
brew install --cask homebrew/cask-versions/adoptopenjdk8
export JAVA_HOME='/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/'
```

If this resolves your issue, remember to add the `JAVA_HOME` environment variable to your bash profile or equivalent shell configuration file.

## Resources

- [Errors Initialising PySpark on Mac](https://www.jscodetips.com/examples/errors-initialising-pyspark-installed-using-pip-on-mac)
- [Spark Installation Guide by Maël Fabien](https://maelfabien.github.io/bigdata/SparkInstall/)
- [Java Download Options](https://java.com/en/download/help/download_options.html)
- [Brian Spiering's Gist on PySpark Installation](https://gist.github.com/brianspiering/1e690b593db025b5acee920fa7330366)
- [Apache Sedona Python Setup](https://sedona.apache.org/1.5.1/setup/install-python/)

Follow these steps carefully for a successful installation of PySpark with Apache Sedona on your M Chip Mac.
