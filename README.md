# SSLCertMonitor

SSLCertMonitor is a Bash script that monitors the SSL certificate status for specified domain names and sends email notifications when the certificate has 15 days or less remaining.

## Features

- SSL certificate expiration monitoring
- Email notifications for certificates expiring in 15 days or less
- Easy configuration

## Requirements

- `curl`: To fetch JSON data from the API
- `jq`: To process JSON data
- `sendmail`: To send emails

## Installation

1. Clone this repository:

    ```bash
    git clone https://github.com/enessahins/SSLCertMonitor.git
    cd SSLCertMonitor
    ```

2. Install the necessary packages (if not already installed):

    ```bash
    sudo apt-get install curl jq sendmail
    ```

3. Configure the necessary email settings in the `send_email` function:

   - `from_email`: The sender's email address.
   - `email_recipients`: The recipient email addresses for notifications.

4. Define the domain names to monitor in the `domains` array.

## Usage

The script can be executed directly from the terminal:

```bash
bash SSLCertMonitor.sh
```


By using cron, you can schedule SSLCertMonitor to check your SSL/TLS certificates regularly and ensure timely notifications for any issues or expirations.

First, edit the crontab file by running:
> crontab -e

Then, add a line to the crontab file like this:
0 0 * * * /path/to/your/sslcertmonitor.sh
