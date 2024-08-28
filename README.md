
# Setting Up MLflow Server on Amazon EC2 with SageMaker

Follow these steps to set up an MLflow server on an EC2 instance. This guide assumes you have access to AWS and the necessary permissions.

## 1. Create an EC2 Instance

1. Log in to your AWS Management Console.
2. Navigate to the EC2 Dashboard.
3. Click on "Launch Instance."
4. Choose your preferred AMI (Amazon Machine Image), such as Ubuntu 20.04 LTS.
5. Select an instance type (e.g., `t2.medium`).
6. Configure instance details, storage, and tags as needed.
7. Set up your security group, allowing SSH (port 22) and HTTP (port 5000) access.
8. Review and launch the instance.
9. Download the key pair (e.g., `my-key.pem`) for SSH access.

## 2. Connect to the EC2 Instance via SSH

Use the following command to connect to your EC2 instance. Replace `your-ec2-user` with `ubuntu` or `ec2-user` depending on your AMI, and `your-ec2-ip` with the public IP address of your instance:

```bash
ssh -i /path/to/your-key.pem your-ec2-user@your-ec2-ip
```

## 3. Install Required Packages

Once connected to the EC2 instance, execute the following commands:

```bash
sudo apt update
sudo apt install python3-pip
sudo apt install pipx
sudo pipx install pipenv
sudo pipx install virtualenv
```

## 4. Set Up the MLflow Environment

1. Create a directory for MLflow:

    ```bash
    mkdir mlflow
    cd mlflow
    ```

2. Install `pipenv` if not already installed:

    ```bash
    sudo apt install pipenv
    ```

3. Install MLflow and AWS CLI tools using `pipenv`:

    ```bash
    pipenv install mlflow
    pipenv install awscli
    pipenv install boto3
    ```

4. Activate the `pipenv` virtual environment:

    ```bash
    pipenv shell
    ```

## 5. Configure AWS CLI

Run the following command to configure your AWS credentials:

```bash
aws configure
```

Enter your `aws_access_key_id`, `aws_secret_access_key`, and the default region name.

## 6. Install `setuptools`

Run the following command:

```bash
pip install setuptools
```

## 7. Start the MLflow Server

Finally, start the MLflow server with the following command. Replace `your-s3-bucket` with the name of your S3 bucket:

```bash
mlflow server -h 0.0.0.0 --default-artifact-root s3://your-s3-bucket
```

Your MLflow server is now running and accessible from the public IP of your EC2 instance on port 5000.

## 8. Access the MLflow UI

Open a web browser and navigate to `http://your-ec2-ip:5000` to access the MLflow tracking UI.

---

**Note:** Ensure your S3 bucket is correctly configured with the necessary permissions for MLflow to store artifacts.
