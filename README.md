# Stack Overflow for Teams API Import (so4t_api_import)
An API script for Stack Overflow for Teams that facilitates bulk-importing questions, answers, or articles from a CSV file.

This script is offered with no formal support from Stack Overflow. If you run into issues using the script, please [open an issue](https://github.com/jklick-so/so4t_api_import/issues) and/or reach out to the person who provided it to you. You are also welcome to edit the script to suit your needs.

## Requirements
* A Stack Overflow for Teams instance (Business or Enterprise)
* Python 3.x ([download](https://www.python.org/downloads/))
* Operating system: Linux, MacOS, or Windows

## Setup

[Download](https://github.com/jklick-so/so4t_api_import/archive/refs/heads/main.zip) and unpack the contents of this repository

**Install Required Python Libraries**

* Open a terminal window (or, for Windows, a command prompt)
* Navigate to the directory where you unpacked the files
* Install the dependencies: `pip3 install -r requirements.txt`

**API Authentication**

For the Business tier you'll need a [personal access token](https://stackoverflowteams.help/en/articles/4385859-stack-overflow-for-teams-api) (PAT). For Enterprise, you'll need to obtain both an API key and an access token. Documentation for creating an Enterprise key and token can be found within your instance, at this url: `https://[your_site]/api/docs/authentication`

Creating an access token for Enterpise can sometimes be tricky for people who haven't done it before. Here are some (hopefully) straightforward instructions:

* Go to the page where you created your API key. Take note of the "Client ID" associated with your API key.
* Go to the following URL, replacing the base URL, the `client_id`, and base URL of the `redirect_uri` with your own:
`https://YOUR.SO-ENTERPRISE.URL/oauth/dialog?client_id=111&redirect_uri=https://YOUR.SO-ENTERPRISE.URL/oauth/login_success&scope=write_access`
* You may be prompted to login to Stack Overflow Enterprise, if you're not already. Either way, you'll be redirected to a page that simply says "Authorizing Application"
* In the URL of that page, you'll find your access token. Example: `https://YOUR.SO-ENTERPRISE.URL/oauth/login_success#access_token=YOUR_TOKEN`

## Basic Usage
First, you'll need to populate a CSV with content to import into Stack Overflow for Teams. There's a [CSV Templates](https://github.com/jklick-so/so4t_api_import/tree/main/CSV%20Templates) directory in this project that you can use as a starting point. The CSV files found therein are preformatted with the proper column names.

Once you have a CSV file created, open a terminal window and navigate to the directory where you unpacked the script. Examples of running the script:
* Importing questions into Business from a CSV file named 'questions.csv': 
`python3 so4t_api_import.py --url "https://stackoverflowteams.com/c/TEAM-NAME" --token "YOUR_TOKEN" --csv 'questions.csv' --questions`
* Importing articles into Enterprise from a CSV file named 'articles.csv': `python3 so4t_api_import.py --url "https://SUBDOMAIN.stackenterprise.co" --key "YOUR_KEY --token "YOUR_TOKEN" --csv 'articles.csv' --articles`

Standard usage of the script will involve using the following arguments:
* `--url https://your.instance.url`
* `--token 'YOUR_TOKEN'`
* `--key 'YOUR_KEY'` [only required for Enterprise]
* `--csv 'path/to/file.csv'`
* `--articles` or `--questions` [one or the other, depending on what you're importing]

Additionally, you can always view available arguments and descriptions via the `--help` argument. Example: `python3 so4t_api_import.py --help`

As the script runs, it will continue to update the terminal window with the tasks it's performing.

## Advanced Usage
For Enterprise customers, there's an advanced ability to impersonate users for the purposes of appropriately attributing imported content to specific users. Documentation of the functionality can be found here: https://support.stackenterprise.co/support/solutions/articles/22000245133-service-keys-identity-delegation-and-impersonation#impersonation

Impersonation functionality is not enabled by default and requires opening a ticket with support@stackoverflow.com. It also requires a user account with admin privileges, both to configure impersonation and to use the impersonation functionality of this API script.

Adding the `--impersonate` argument to the basic usage of the script allows you to leverage the impersonation functionality. You'll need to use an impersonation CSV format, which includes additional columns for the account IDs of the users to impersonate. Please see the [CSV Templates](https://github.com/jklick-so/so4t_api_import/tree/main/CSV%20Templates) directory and use a template with the 'impersonation' prefix.

## Known limitations and considerations

* Articles can only be an imported at a rate of one per minute. Yes, it's slow. It's a limitation of v2.3 of the API. API v3 does not have this limitation and it's the version of the API the script uses for importing questions and answers, which is much faster. However, API v3 is still a work in progress and does not yet support Articles.
* Source content that contains images, embeds, and attachments cannot be imported via API. Images can be imported by copy-paste after the bulk import is completed. Embeds and attachments can be likewise added post-import by either copying their contents or hyperlinking to the external file. 
* Question imports are currently designed to support importing a single, corresponding answer. This is on the backlog for improvement.

If you encounter any major hurdles -- known or otherwise -- please [report them](https://github.com/jklick-so/so4t_api_import/issues) so improvements can be made, or feel free to make your own changes as needed.
