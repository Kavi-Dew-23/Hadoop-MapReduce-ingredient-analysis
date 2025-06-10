# Ingredient Frequency Counter using Hadoop MapReduce

## 📌 Overview
This project uses Apache Hadoop MapReduce to analyze a dataset of ingredients and count how frequently each one appears.

## 🔧 Technologies
- Java
- Apache Hadoop (HDFS, MapReduce)
- Ubuntu (WSL)
- Dataset: `top_ingredients.txt`

  ## 🗂️ Files
- `IngredientFrequency.java`: Main MapReduce logic java file
- `https://www.kaggle.com/datasets/wilmerarltstrmberg/recipe-dataset-over-2m?select=recipes_data.csv`: Input file

  ## 🚀 How to Run
   Install WSL and Ubuntu
  # on windows
  wsl --install
  after installation:
  wsl

  #  Install Java in WSL
  sudo apt update
sudo apt install openjdk-11-jdk -y
java -version

 # Download and Install Hadoop in WSL
 cd ~
wget https://downloads.apache.org/hadoop/common/hadoop-2.10.2/hadoop-2.10.2.tar.gz
tar -xvzf hadoop-2.10.2.tar.gz
mv hadoop-2.10.2 hadoop

# Set Environment Variables
nano ~/.bashrc

Add to the bottom:
export HADOOP_HOME=~/hadoop
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

Apply changes:
source ~/.bashrc

#  Set Java Path in Hadoop Config
nano ~/hadoop/etc/hadoop/hadoop-env.sh
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

#  Format HDFS and Start Hadoop
hdfs namenode -format
start-dfs.sh
start-yarn.sh

If SSH fails, run:
sudo apt install openssh-server -y
ssh-keygen -t rsa -P ""
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

Then restart:
start-dfs.sh
start-yarn.sh

# Create Project Folder
mkdir ~/ingredient-count
cd ~/ingredient-count

#  Save Java Code
nano IngredientFrequency.java

# Compile and Package
javac -classpath $(hadoop classpath) -d . IngredientFrequency.java
jar -cvf ingredientcount.jar *.class

# Copy Dataset to WSL
mkdir ~/datasets
cp "/mnt/c/Users/ASUS/Documents/MapReduce/recipes_data.csv" ~/datasets/

# Step 1: Upload input to HDFS
hdfs dfs -mkdir -p /input
hdfs dfs -put ~/datasets/recipes_data.csv /input/

# Step 2: Run MapReduce
hadoop jar IngredientCount.jar IngredientCount /input /output

# Step 3: View results
hdfs dfs -cat /output/part-r-00000
