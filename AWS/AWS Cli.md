---
date created: 2025-10-28 10:09
---

Setting up the AWS CLI (Command Line Interface) on your local machine involves configuring it with your AWS credentials.

### Prerequisites:

1. **Install the AWS CLI**:
   - **macOS / Linux**: You can install the AWS CLI via Homebrew or directly with the official package.
     - Homebrew (macOS):   
```bash
    brew install awscli
```

- Linux: Use the installation guide for your distribution from [AWS CLI installation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).
- **Windows**: You can use the installer from the [AWS CLI website](https://aws.amazon.com/cli/).
2. **Ensure you have an AWS account**: You need an AWS account to get the access keys.

---

### Step 1: Open Terminal or Command Prompt

- On **macOS/Linux**, open your terminal.
- On **Windows**, open Command Prompt or PowerShell.

---

### Step 2: Run `aws configure`

To configure AWS CLI, run the following command in the terminal:

```bash
aws configure
```

This will prompt you for the following information:

#### 1. **AWS Access Key ID**:

This is the access key ID for your AWS account. You can get it from the AWS Management Console by following these steps:

- Go to [IAM (Identity and Access Management)](https://console.aws.amazon.com/iam/).
- In the left panel, select **Users**, then click on your username.
- Under the **Security Credentials** tab, create or view your **Access Keys**.

#### 2. **AWS Secret Access Key**:

This is the secret access key corresponding to the Access Key ID. When you generate a new access key, you'll be able to download the secret access key, so make sure to store it somewhere safe.

#### 3. **Default Region Name**:

You need to set a default region (e.g., `us-east-1`, `us-west-2`, etc.). This will be used for API requests unless you specify a different region in individual AWS commands.

Example:

```bash
us-east-1
```

#### 4. **Default Output Format**:

The output format is optional. You can choose between:

- `json` (default)
- `text
- `table`

If you don’t want to use any specific output format, just hit **Enter** to keep the default (`json`).

---

### Step 3: Verify the Configuration

Once you’ve completed the configuration, you can verify that it’s working by running a simple AWS command. For example:

```bash
aws s3 ls
```

This will list your S3 buckets (if any). If everything is set up correctly, you'll see a list of your S3 buckets, or an empty list if you don’t have any yet.

---
