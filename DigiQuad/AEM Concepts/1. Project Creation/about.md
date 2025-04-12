# AEM Project Setup Guide

This guide helps you set up a new Adobe Experience Manager (AEM) project using the provided configuration and archetype files.

---

## 1. Initial Setup

- When you first access the CRX/DE instance, it will look empty. Also it will have an extra icon along with Develop and Package button.
- Install the configuration file provided with the installer ZIP package shared by the dev team.

Reference images:

<p align="center">
  <img src="https://github.com/user-attachments/assets/bc8f5e1e-e472-427a-9af1-3ef0e5b8b9de" alt="Initial CRXDE View" width="300" style="margin-right: 20px;"/>
  <img src="https://github.com/user-attachments/assets/c6bbf2ee-b1ce-4367-8f2d-dfdcafd50c5a" alt="Installer ZIP Files" width="400"/>
</p>

---

## 2. Post Installation

After the configuration file is installed:

- You will see two icons:
  - **Develop**
  - **Package**

These indicate the setup has been completed successfully.

---

## 3. Create a New Project

1. Create a new folder where you want the AEM project to reside.

2. Open a terminal inside that folder.

3. Run the following Maven command to generate your AEM project:

    ```bash
    mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
      -DarchetypeGroupId=com.adobe.aem \
      -DarchetypeArtifactId=aem-project-archetype \
      -DarchetypeVersion=41 \
      -DappTitle="First Project" \
      -DappId="Sample-Project-Sibaram" \
      -DgroupId="com.sibaram" \
      -DaemVersion="6.5.13" \
      -DincludeDispatcherConfig="y" \
      -Dlanguage="en" \
      -Dcountry="us" \
      -DsingleCountry="n"
    ```

    Change `-DappTitle=""`  `-DappId=""` and `-DgroupId=""` as per your project

4. After the build succeeds, go to CRXDE and check if the project folder structure has been created.

<p align="center">
  <img src="https://github.com/user-attachments/assets/17df67c6-23e7-4593-b4ad-1b5d613483af" alt="CRXDE Folder Structure" width="300"/>
</p>

5. Navigate into your project folder and run the following command to build and deploy to your AEM Author instance:

    ```bash
    mvn clean install -PautoInstallPackage
    ```

    - After successful installation, your project will appear in the AEM Author environment.

<p align="center">
  <img src="https://github.com/user-attachments/assets/a5ef650b-545d-4cc9-a80f-d9bda8009270" alt="AEM Author Instance View" width="300"/>
</p>

---
