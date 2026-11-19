# ansiblejava

Write a Java program to read marks in three subjects, calculate the average, and display the grade according to the following criteria: A (90 and above), B (75–89), C (50–74), and F (below 50). Create a Maven project and configure an Azure Pipeline to compile the application and generate the JAR file.
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter mark in Subject 1: ");
        int m1 = sc.nextInt();

        System.out.print("Enter mark in Subject 2: ");
        int m2 = sc.nextInt();

        System.out.print("Enter mark in Subject 3: ");
        int m3 = sc.nextInt();

        double average = (m1 + m2 + m3) / 3.0;

        System.out.println("Average: " + average);

        if (average >= 90)
            System.out.println("Grade: A");
        else if (average >= 75)
            System.out.println("Grade: B");
        else if (average >= 50)
            System.out.println("Grade: C");
        else
            System.out.println("Grade: F");

        sc.close();
    }
}
Pom.xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>GradeProject</artifactId>
    <version>1.0</version>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.13.0</version>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>3.4.2</version>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>Main</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

azure-pipelines.yml 
trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- task: JavaToolInstaller@0
  inputs:
    versionSpec: '17'
    jdkArchitectureOption: 'x64'
    jdkSourceOption: 'PreInstalled'

- task: Maven@3
  inputs:
    mavenPomFile: 'pom.xml'
    goals: 'clean package'
    publishJUnitResults: false
    javaHomeOption: 'JDKVersion'
    jdkVersionOption: '1.17'
    mavenVersionOption: 'Default'

- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: 'target'
    ArtifactName: 'JavaBuild'
    publishLocation: 'Container'



java -jar target/GradeProject-1.0.jar
To test jar file





Develop a Java application to calculate an electricity bill based on the units consumed, using the following rates: ₹1.50 per unit for the first 100 units, ₹2.50 per unit for the next 100 units, and ₹4.00 per unit for units exceeding 200. Build the application as a Maven project and configure an Azure Pipeline to compile the source code and generate the JAR artifact.
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter units consumed: ");
        int units = sc.nextInt();

        double bill;

        if (units <= 100) {
            bill = units * 1.50;
        }
        else if (units <= 200) {
            bill = (100 * 1.50) + (units - 100) * 2.50;
        }
        else {
            bill = (100 * 1.50) + (100 * 2.50)
                   + (units - 200) * 4.00;
        }

        System.out.println("Electricity Bill: Rs." + bill);

        sc.close();
    }
}



Develop a Java application to calculate simple interest based on the principal amount, rate of interest, and time period provided as input. Build the application as a Maven project and configure an Azure Pipeline to compile the source code and generate the JAR artifact.
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter principal amount: ");
        double principal = sc.nextDouble();

        System.out.print("Enter rate of interest: ");
        double rate = sc.nextDouble();

        System.out.print("Enter time in years: ");
        double time = sc.nextDouble();

        double interest = (principal * rate * time) / 100;

        System.out.println("Simple Interest: " + interest);
        System.out.println("Total Amount: " + (principal + interest));

        sc.close();
    }
}
Develop a Java application to calculate the area of a circle based on its radius provided as input. Build the application as a Maven project and configure an Azure Pipeline to compile the source code and generate the JAR artifact.
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter radius: ");
        double radius = sc.nextDouble();

        double area = Math.PI * radius * radius;

        System.out.println("Area of Circle: " + area);

        sc.close();
    }
}

trigger:
- main

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: Maven@3
  inputs:
    mavenPomFile: 'pom.xml'
    mavenOptions: '-Xmx3072m'
    javaHomeOption: 'JDKVersion'
    jdkVersionOption: '17'
    jdkArchitectureOption: 'x64'
    goals: 'clean test'

- task: PublishTestResults@2
  inputs:
    testResultsFiles: '**/surefire-reports/TEST-*.xml'
    testRunTitle: 'JUnit Test Results'





Develop a Java calculator application to perform addition, subtraction, multiplication, and division, including division-by-zero handling. Write JUnit test cases for all arithmetic operations and configure an Azure Pipeline using Maven to build the application and run the regression tests

package org.example;

public class Calculator {

    public int add(int a, int b) {
        return a + b;
    }

    public int subtract(int a, int b) {
        return a - b;
    }

    public int multiply(int a, int b) {
        return a * b;
    }

    public double divide(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("Division by zero");
        }
        return (double) a / b;
    }
}

package org.example;

import org.junit.Test;
import static org.junit.Assert.*;

public class CalculatorTest {

    Calculator calculator = new Calculator();

    @Test
    public void testAddition() {
        assertEquals(10, calculator.add(5, 5));
    }

    @Test
    public void testSubtraction() {
        assertEquals(5, calculator.subtract(10, 5));
    }

    @Test
    public void testMultiplication() {
        assertEquals(25, calculator.multiply(5, 5));
    }

    @Test
    public void testDivision() {
        assertEquals(2.0, calculator.divide(10, 5), 0.001);
    }

    @Test(expected = ArithmeticException.class)
    public void testDivisionByZero() {
        calculator.divide(10, 0);
    }
}
Develop a Java application to determine whether a student has passed or failed based on the marks obtained (Pass: marks ≥ 50; Fail: marks < 50). Write JUnit test cases for marks 35, 50, 75, and 100, and configure an Azure Pipeline using Maven to build the application and run the regression tests.


package org.example;

public class StudentResult {

    public String checkResult(int marks) {

        if (marks >= 50) {
            return "Pass";
        } else {
            return "Fail";
        }
    }

    public static void main(String[] args) {

        StudentResult student = new StudentResult();

        int marks = 75;

        System.out.println(student.checkResult(marks));
    }
}


package org.example;

import org.junit.Test;
import static org.junit.Assert.assertEquals;

public class StudentResultTest {

    StudentResult student = new StudentResult();

    @Test
    public void testFail35() {
        assertEquals("Fail", student.checkResult(35));
    }

    @Test
    public void testPass50() {
        assertEquals("Pass", student.checkResult(50));
    }

    @Test
    public void testPass75() {
        assertEquals("Pass", student.checkResult(75));
    }

    @Test
    public void testPass100() {
        assertEquals("Pass", student.checkResult(100));
    }
}



Develop a Java application to determine whether a given year is a leap year. Write JUnit test cases covering leap years, non-leap years, and century years, and configure an Azure Pipeline using Maven to build the application and run the regression tests.


package org.example;

public class LeapYear {

    public boolean isLeapYear(int year) {

        if (year % 400 == 0) {
            return true;
        } 
        else if (year % 100 == 0) {
            return false;
        } 
        else if (year % 4 == 0) {
            return true;
        } 
        else {
            return false;
        }
    }

    public static void main(String[] args) {

        LeapYear obj = new LeapYear();

        int year = 2024;

        if (obj.isLeapYear(year)) {
            System.out.println("Leap Year");
        } else {
            System.out.println("Not a Leap Year");
        }
    }
}


package org.example;

import org.junit.Test;
import static org.junit.Assert.assertTrue;
import static org.junit.Assert.assertFalse;

public class LeapYearTest {

    LeapYear obj = new LeapYear();

    @Test
    public void testLeapYear2024() {
        assertTrue(obj.isLeapYear(2024));
    }

    @Test
    public void testNonLeapYear2023() {
        assertFalse(obj.isLeapYear(2023));
    }

    @Test
    public void testCenturyYear1900() {
        assertFalse(obj.isLeapYear(1900));
    }

    @Test
    public void testCenturyYear2000() {
        assertTrue(obj.isLeapYear(2000));
    }

    @Test
    public void testLeapYear2020() {
        assertTrue(obj.isLeapYear(2020));
    }
}

Develop a Java application to calculate the final purchase amount by applying a 10% discount when the purchase amount is ₹1,000 or more; otherwise, no discount is applied. Write JUnit test cases for purchase amounts below, equal to, and above ₹1,000, and configure an Azure Pipeline using Maven to build the application and run the regression tests.

package org.example;

public class DiscountCalculator {

    public double calculateFinalAmount(double amount) {

        if (amount >= 1000) {
            return amount - (amount * 0.10);
        } else {
            return amount;
        }
    }

    public static void main(String[] args) {

        DiscountCalculator obj = new DiscountCalculator();

        double amount = 1500;

        System.out.println("Final Amount: " +
                obj.calculateFinalAmount(amount));
    }
}


package org.example;

import org.junit.Test;
import static org.junit.Assert.assertEquals;

public class DiscountCalculatorTest {

    DiscountCalculator obj = new DiscountCalculator();

    @Test
    public void testBelow1000() {
        assertEquals(900.0, obj.calculateFinalAmount(900), 0.001);
    }

    @Test
    public void testEqualTo1000() {
        assertEquals(900.0, obj.calculateFinalAmount(1000), 0.001);
    }

    @Test
    public void testAbove1000() {
        assertEquals(1350.0, obj.calculateFinalAmount(1500), 0.001);
    }
}

You mean all 4 Maven regression-testing experiments. Here are the Java application and JUnit test codes for all four.
Use the same package name org.example in both main and test folders. Keep the same JUnit 4.13.2 dependency and Azure YAML pipeline.
1. Student Pass or Fail
StudentResult.java — src/main/java/org/example/StudentResult.java
package org.example;

public class StudentResult {

    public String checkResult(int marks) {

        if (marks >= 50) {
            return "Pass";
        } else {
            return "Fail";
        }
    }

    public static void main(String[] args) {

        StudentResult obj = new StudentResult();

        int marks = 75;

        System.out.println(obj.checkResult(marks));
    }
}
StudentResultTest.java — src/test/java/org/example/StudentResultTest.java
package org.example;

import org.junit.Test;
import static org.junit.Assert.assertEquals;

public class StudentResultTest {

    StudentResult obj = new StudentResult();

    @Test
    public void test35() {
        assertEquals("Fail", obj.checkResult(35));
    }

    @Test
    public void test50() {
        assertEquals("Pass", obj.checkResult(50));
    }

    @Test
    public void test75() {
        assertEquals("Pass", obj.checkResult(75));
    }

    @Test
    public void test100() {
        assertEquals("Pass", obj.checkResult(100));
    }
}
4 tests
2. Leap Year
LeapYear.java — src/main/java/org/example/LeapYear.java
package org.example;

public class LeapYear {

    public boolean isLeapYear(int year) {

        if (year % 400 == 0) {
            return true;
        }
        else if (year % 100 == 0) {
            return false;
        }
        else if (year % 4 == 0) {
            return true;
        }
        else {
            return false;
        }
    }

    public static void main(String[] args) {

        LeapYear obj = new LeapYear();

        int year = 2024;

        if (obj.isLeapYear(year)) {
            System.out.println("Leap Year");
        } else {
            System.out.println("Not a Leap Year");
        }
    }
}
LeapYearTest.java — src/test/java/org/example/LeapYearTest.java
package org.example;

import org.junit.Test;
import static org.junit.Assert.assertTrue;
import static org.junit.Assert.assertFalse;

public class LeapYearTest {

    LeapYear obj = new LeapYear();

    @Test
    public void testLeapYear2024() {
        assertTrue(obj.isLeapYear(2024));
    }

    @Test
    public void testNonLeapYear2023() {
        assertFalse(obj.isLeapYear(2023));
    }

    @Test
    public void testCenturyYear1900() {
        assertFalse(obj.isLeapYear(1900));
    }

    @Test
    public void testCenturyYear2000() {
        assertTrue(obj.isLeapYear(2000));
    }

    @Test
    public void testLeapYear2020() {
        assertTrue(obj.isLeapYear(2020));
    }
}
5 tests
3. Discount Calculator
DiscountCalculator.java — src/main/java/org/example/DiscountCalculator.java
package org.example;

public class DiscountCalculator {

    public double calculateFinalAmount(double amount) {

        if (amount >= 1000) {
            return amount - (amount * 0.10);
        } else {
            return amount;
        }
    }

    public static void main(String[] args) {

        DiscountCalculator obj = new DiscountCalculator();

        double amount = 1500;

        System.out.println("Purchase Amount: " + amount);
        System.out.println("Final Amount: " +
                obj.calculateFinalAmount(amount));
    }
}
DiscountCalculatorTest.java — src/test/java/org/example/DiscountCalculatorTest.java
package org.example;

import org.junit.Test;
import static org.junit.Assert.assertEquals;

public class DiscountCalculatorTest {

    DiscountCalculator obj = new DiscountCalculator();

    @Test
    public void testBelow1000() {
        assertEquals(900.0, obj.calculateFinalAmount(900), 0.001);
    }

    @Test
    public void testEqualTo1000() {
        assertEquals(900.0, obj.calculateFinalAmount(1000), 0.001);
    }

    @Test
    public void testAbove1000() {
        assertEquals(1350.0, obj.calculateFinalAmount(1500), 0.001);
    }
}
3 tests
4. Calculator
Calculator.java — src/main/java/org/example/Calculator.java
package org.example;

public class Calculator {

    public int add(int a, int b) {
        return a + b;
    }

    public int subtract(int a, int b) {
        return a - b;
    }

    public int multiply(int a, int b) {
        return a * b;
    }

    public double divide(int a, int b) {

        if (b == 0) {
            throw new ArithmeticException("Division by zero");
        }

        return (double) a / b;
    }

    public static void main(String[] args) {

        Calculator obj = new Calculator();

        System.out.println("Addition: " + obj.add(10, 5));
        System.out.println("Subtraction: " + obj.subtract(10, 5));
        System.out.println("Multiplication: " + obj.multiply(10, 5));
        System.out.println("Division: " + obj.divide(10, 5));
    }
}
CalculatorTest.java — src/test/java/org/example/CalculatorTest.java
package org.example;

import org.junit.Test;
import static org.junit.Assert.*;

public class CalculatorTest {

    Calculator obj = new Calculator();

    @Test
    public void testAddition() {
        assertEquals(15, obj.add(10, 5));
    }

    @Test
    public void testSubtraction() {
        assertEquals(5, obj.subtract(10, 5));
    }

    @Test
    public void testMultiplication() {
        assertEquals(50, obj.multiply(10, 5));
    }

    @Test
    public void testDivision() {
        assertEquals(2.0, obj.divide(10, 5), 0.001);
    }

    @Test(expected = ArithmeticException.class)
    public void testDivisionByZero() {
        obj.divide(10, 0);
    }
}
5 tests
Common configuration for all four programs
JUnit dependency in pom.xml — same for all four:
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
        <scope>test</scope>
    </dependency>
</dependencies>
Azure Pipeline YAML — same for all four (assuming your GitHub branch is master):
trigger:
- master

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: Maven@3
  inputs:
    mavenPomFile: 'pom.xml'
    mavenOptions: '-Xmx3072m'
    javaHomeOption: 'JDKVersion'
    jdkVersionOption: '17'
    jdkArchitectureOption: 'x64'
    goals: 'clean test'

- task: PublishTestResults@2
  inputs:
    testResultsFiles: '**/surefire-reports/TEST-*.xml'
    testRunTitle: 'Regression Test Results'
Expected test summary
Experiment
Number of tests
Student Pass/Fail
4
Leap Year
5
Discount Calculator
3
Calculator
5
Total
17

Important lab instructions:
Create a separate Maven project for each experiment.
Use the corresponding application class in src/main/java.
Use the corresponding test class in src/test/java.
Run the test Java class in IntelliJ.
Push the code to its GitHub repository.
Create/run the Azure Maven pipeline.
Check the Tests tab for successful execution.
You can reuse the dependency and YAML configuration across all four experiments.


# Update package index
sudo apt update

# Install Java and fontconfig
sudo apt install fontconfig openjdk-21-jdk -y

# Create Jenkins keyring directory
sudo mkdir -p /etc/apt/keyrings

# Download Jenkins key
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key

# Add Jenkins repository
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

# Update package index
sudo apt update

# Install Jenkins
sudo apt-get install jenkins -y

# Configure Java version
sudo update-alternatives --config java

# Start Jenkins
sudo systemctl start jenkins

# Enable Jenkins
sudo systemctl enable jenkins

# Check Jenkins status
sudo systemctl status jenkins

# Get initial Jenkins administrator password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Yes! Here are all four Jenkins pipelines using direct Java files (without Maven) with three stages: Checkout, Build and Execute.
1. Palindrome
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/YOUR_USERNAME/Palindrome-Jenkins-CI.git'
            }
        }

        stage('Build') {
            steps {
                sh 'javac Palindrome.java'
            }
        }

        stage('Execute') {
            steps {
                sh 'echo 121 | java Palindrome'
            }
        }
    }
}
Expected output: Palindrome Number
2. Prime Number
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/YOUR_USERNAME/Prime-Jenkins-CI.git'
            }
        }

        stage('Build') {
            steps {
                sh 'javac Prime.java'
            }
        }

        stage('Execute') {
            steps {
                sh 'echo 7 | java Prime'
            }
        }
    }
}
Expected output: Prime Number
3. Factorial
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/YOUR_USERNAME/Factorial-Jenkins-CI.git'
            }
        }

        stage('Build') {
            steps {
                sh 'javac Factorial.java'
            }
        }

        stage('Execute') {
            steps {
                sh 'echo 5 | java Factorial'
            }
        }
    }
}
Expected output: Factorial = 120
4. Fibonacci
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/YOUR_USERNAME/Fibonacci-Jenkins-CI.git'
            }
        }

        stage('Build') {
            steps {
                sh 'javac Fibonacci.java'
            }
        }

        stage('Execute') {
            steps {
                sh 'echo 7 | java Fibonacci'
            }
        }
    }
}
Expected output: 0 1 1 2 3 5 8
Common procedure
Upload the .java file to GitHub.
Open Jenkins.
Click New Item → Pipeline.
Enter the job name.
Select Pipeline script.
Paste the respective code.
Click Apply → Save.
Click Build Now.
Open Console Output.
Remember these three commands:
Stage
Command
Purpose
Checkout
git
Download code
Build
javac Filename.java
Compile code
Execute
echo input \| java ClassName
Run code

Same three-stage structure for all four programs
Just replace YOUR_USERNAME with your GitHub username before running.
Here are all four Java programs for your Jenkins CI experiments. These use Scanner, loops and simple logic, without Maven.
1. Palindrome Number
File name: Palindrome.java
import java.util.Scanner;

public class Palindrome {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter a number: ");
        int num = sc.nextInt();

        int original = num;
        int reverse = 0;

        while (num > 0) {
            int digit = num % 10;
            reverse = reverse * 10 + digit;
            num = num / 10;
        }

        if (original >= 0 && original == reverse) {
            System.out.println("Palindrome Number");
        } else {
            System.out.println("Not a Palindrome Number");
        }

        sc.close();
    }
}
Output:
Enter a number: 121
Palindrome Number
2. Prime Number
File name: Prime.java
import java.util.Scanner;

public class Prime {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter a number: ");
        int num = sc.nextInt();

        boolean prime = true;

        if (num <= 1) {
            prime = false;
        } else {
            for (int i = 2; i <= num / 2; i++) {
                if (num % i == 0) {
                    prime = false;
                    break;
                }
            }
        }

        if (prime) {
            System.out.println("Prime Number");
        } else {
            System.out.println("Not a Prime Number");
        }

        sc.close();
    }
}
Output:
Enter a number: 7
Prime Number
3. Factorial
File name: Factorial.java
import java.util.Scanner;

public class Factorial {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter a number: ");
        int num = sc.nextInt();

        long fact = 1;

        for (int i = 1; i <= num; i++) {
            fact = fact * i;
        }

        System.out.println("Factorial = " + fact);

        sc.close();
    }
}
Output:
Enter a number: 5
Factorial = 120
4. Fibonacci Series
File name: Fibonacci.java
import java.util.Scanner;

public class Fibonacci {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter the number of terms: ");
        int n = sc.nextInt();

        int a = 0, b = 1;

        System.out.println("Fibonacci Series:");

        for (int i = 1; i <= n; i++) {
            System.out.print(a + " ");

            int c = a + b;
            a = b;
            b = c;
        }

        sc.close();
    }
}
Output:
Enter the number of terms: 7
Fibonacci Series:
0 1 1 2 3 5 8
Important: Commands for Jenkins
Use these commands in the respective pipeline stages.
Program
Build command
Execute command
Palindrome
javac Palindrome.java
echo 121 \| java Palindrome
Prime
javac Prime.java
echo 7 \| java Prime
Factorial
javac Factorial.java
echo 5 \| java Factorial
Fibonacci
javac Fibonacci.java
echo 7 \| java Fibonacci

Remember: The Java filename and public class name must match exactly. Upload each .java file to its corresponding GitHub repository.
# 1. Install Gradle
sudo snap install gradle --classic

# 2. Check Gradle version
gradle -v

# 3. Create project directory
mkdir java_application

# 4. Enter project directory
cd java_application

# 5. Initialize Gradle project
gradle init

# Select:
# Project type: Application
# Language: Java
# Java version: 17
# Project name: java_application
# Application structure: Default
# DSL: Groovy
# Testing framework: JUnit 4
# New APIs: Yes

# 6. Install tree
sudo apt install tree -y

# 7. Display project structure
tree

# 8. Open project in IntelliJ IDEA
idea .

# 9. Edit Java program in IntelliJ:
# src/main/java/demo/App.java
# Keep package demo and class App

# 10. Run Java application
./gradlew run

# 11. Build project
./gradlew build

# 12. Generate JAR file
./gradlew jar

# 13. Execute JAR file
java -jar build/libs/java_application.jar

# 14. Display project structure again
tree

# 15. Initialize Git
git init

# 16. Add all files
git add .

# 17. Check Git status
git status

# 18. Commit files
git commit -m "initial commit"

# 19. Connect GitHub repository
git remote add origin https://github.com/YOUR_USERNAME/Gradledemo.git

# 20. Push to GitHub
git push -u origin master
Since you have already completed the previous experiment, this question is the same experiment with the procedure written as a task statement. You can use your existing work directly.
Ex. No. 8 — Install Ansible and Configure Ansible Roles and Write Playbooks
Aim:
To install Ansible on a Linux system, create three Ansible roles named prerequisites, PostgreSQL, and apache to install Git, PostgreSQL, and Apache respectively, configure the tasks, execute them using a stack.yml playbook on localhost, and verify successful installation.
Procedure
Update the package index:

sudo apt update

Install the required packages and add the Ansible repository:

sudo apt upgrade
sudo apt install -y software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install -y ansible

Verify Ansible installation:

ansible --version

Navigate to the Ansible directory and check the configuration files:

cd /etc/ansible
ls -la

Create the three roles:

sudo ansible-galaxy init prerequisites
sudo ansible-galaxy init PostgreSQL
sudo ansible-galaxy init apache

Configure the role tasks.
prerequisites/tasks/main.yml

---
- name: Install Git
  apt:
    name: git
    state: present
    update_cache: yes

PostgreSQL/tasks/main.yml

---
- name: Install PostgreSQL
  apt:
    name: postgresql
    state: present
    update_cache: yes

- name: Start PostgreSQL service
  systemd:
    name: postgresql
    state: started
    enabled: yes

apache/tasks/main.yml

---
- name: Install Apache web server
  apt:
    name: apache2
    state: present
    update_cache: yes

Create the stack.yml playbook:

---
- hosts: localhost
  connection: local
  become: true

  roles:
    - prerequisites
    - PostgreSQL
    - apache

Execute the playbook:

sudo ansible-playbook stack.yml

Verify the installation:

git --version
apache2 -v
psql --version
sudo systemctl status postgresql
sudo systemctl status apache2
Result
Thus, Ansible was successfully installed, three roles (prerequisites, PostgreSQL, and apache) were created and configured, and the stack.yml playbook was executed on localhost to install and verify Git, PostgreSQL, and Apache web server successfully.
Ex. No: 8 – Configure a Simple Web Application Infrastructure Using Ansible
Aim:
To create an Ansible playbook to configure a simple web application infrastructure on a Linux system by installing Apache, starting and enabling the service, deploying an HTML page, and verifying its availability through HTTP.
Procedure
Step 1: Create a directory and navigate into it.
mkdir -p ~/ansible-webapp
cd ~/ansible-webapp
Step 2: Create the Ansible playbook.
nano webapp.yml
Step 3: Add the following playbook content.

---
- name: Configure Simple Web Application
  hosts: localhost
  connection: local
  become: true

  tasks:
    - name: Install Apache web server
      ansible.builtin.apt:
        name: apache2
        state: present
        update_cache: true

    - name: Start and enable Apache service
      ansible.builtin.service:
        name: apache2
        state: started
        enabled: true

    - name: Deploy HTML web page
      ansible.builtin.copy:
        dest: /var/www/html/index.html
        content: |
          <!DOCTYPE html>
          <html>
          <head>
              <title>My Web Application</title>
          </head>
          <body>
              <h1>Welcome to My Web Application!</h1>
              <p>This web application is deployed successfully
              using Ansible on a Linux system.</p>
          </body>
          </html>
        mode: "0644"

    - name: Verify web application using HTTP request
      ansible.builtin.uri:
        url: http://localhost
        status_code: 200
      register: web_result

    - name: Display deployment status
      ansible.builtin.debug:
        msg: "Web application is accessible successfully!"
Save the file using Ctrl + O, press Enter, and exit using Ctrl + X.
Step 4: Execute the Ansible playbook.
ansible-playbook webapp.yml --ask-become-pass
Enter the Linux password when prompted.
Step 5: Check the Apache service status.
sudo systemctl status apache2
Expected status:
Active: active (running)
Step 6: Verify the web application.
Execute:
curl http://localhost
Expected output:
<h1>Welcome to My Web Application!</h1>
<p>This web application is deployed successfully
using Ansible on a Linux system.</p>
Step 7: Open the web application in a browser.
Open:
http://localhost
The welcome page should be displayed.
Result
Thus, the Ansible playbook was successfully created and executed to install Apache, start and enable the web server, deploy an HTML page containing a heading and description, and verify its accessibility through http://localhost.

