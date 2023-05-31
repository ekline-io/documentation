# EkLine CLI Documentation

Created: April 28, 2023 11:25 PM

The EkLine CLI is a command-line tool designed for seamless document quality control. This guide explains the options available and provides examples for using the EkLine CLI effectively.

## **Installation**

Download the EkLine binary suitable for your operating system from this **[link](https://drive.google.com/drive/folders/11Z75YpLBNq8_EAw_5734YMOTopLwNj1a?usp=share_link)**.

## **Usage**

To use the EkLine CLI, run the following command:

```bash
ekline-cli [options]
```

### **Options**

Below are the options that can be used with EkLine CLI:

- **v, --version**
    
    Displays the current version of EkLine CLI.
    
- **d, --debug**
    
    Enables verbose logging for detailed debug information.
    
- **o, --output `<output-file-name>`**
    
    Specifies the output file name. If not provided, output is displayed in the terminal. For JSON output, end the file name with **`.jsonl`**. For CSV output, end the file name with **`.csv`**.
    
- **ns, --no-suggestion**
    
    Disables suggestions. Note, suggestions are only generated for **`.jsonl`** or **`.csv`** output formats.
    
- **i, --ignore `<rules>`**
    
    Ignores the specified rules during the check. Rules should be passed as comma-separated values. For example, **`EK1,EK4`**.
    
- **cd, --content-directory `<contentDirectory...>`**
    
    Specifies directory paths where the content files are located. Paths should be relative to the current directory.
    
- **et, --ek-token `<token>`**
    
    Provides your Ekline.io API token for authorization. This is a mandatory field.
    
- **h, --help**
    
    Displays help for command.
    

## **Examples**

Here are a few examples of using EkLine CLI:

1. **Running EkLine CLI and ignoring specific rules**

```bash
ekline-cli -i EK1,EK4 --ek-token YOUR_EKLINE_TOKEN
```

1. **Running EkLine CLI and specifying a content directory**

```bash
ekline-cli -cd ./content --ek-token YOUR_EKLINE_TOKEN
```

Remember to replace **`YOUR_EKLINE_TOKEN`** with your actual Ekline.io API token in these examples.