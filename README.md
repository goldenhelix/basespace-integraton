# BaseSpace Automation Tasks

This repository contains Golden Helix server tasks for automating file downloads from Illumina BaseSpace.

## Prerequisites

- Access to Illumina BaseSpace
- Valid BaseSpace account credentials

## Setup Instructions

### 1. Get BaseSpace API Token

This step requires access to a Linux machine, but can also be done through the VSCode app on the Golden Helix server by going to *Terminal > New Terminal* and running the following commands:

1. Download the BaseSpace CLI tools from the [BaseSpace CLI Overview](https://developer.basespace.illumina.com/docs/content/documentation/cli/cli-overview)
2. After installing the CLI tools, authenticate by running:
   ```sh
   cd ~
   wget -q "https://launch.basespace.illumina.com/CLI/latest/amd64-linux/bs" -O ./bs
   chmod u+x ./bs
   ./bs auth
   ```
3. You will be prompted to login to BaseSpace by copying and pasting the URL into your browser. Once logged in, the command will complete.

4. Retrieve the token from `~/.basespace/default.cfg`. This token will be used as the "BaseSpace API key"

    ```sh
    cat ~/.basespace/default.cfg
    ```

The token will be displayed in the output. It will look like this:

```
apiServer   = https://api.basespace.illumina.com
accessToken = 9f5b6f49d1001a46e2c6fd791bfe791ce
```

Edit the `basespace_project.task.yaml` and `basespace_biosample.task.yaml` files and replace the `basespace_access_token` with the token you just retrieved.

If the API server in your `default.cfg` file is different from the default shown above, make sure to update the `BASESPACE_API_SERVER` variable in both task files to match your specific API server URL.

## Tasks

The repository contains the following tasks:

### Download BaseSpace Project Files (`basespace_project.task.yaml`)

Downloads files from a specific BaseSpace project.

Parameters:
- `project`: BaseSpace Project Name (example: 'MiniSeq: BRCA Panel')
- `output_directory`: Directory where files will be downloaded
- `file_filter`: Optional file extension filter (default: 'vcf.gz')
- `basespace_access_token`: Your BaseSpace API key

### Download BaseSpace BioSample (`basespace_biosample.task.yaml`)

Downloads files for a specific BioSample from BaseSpace.

Parameters:
- `bio_sample`: BaseSpace BioSample Name (example: 'Veriti2-OBRCA-NA12878-20ng-rep4')
- `output_directory`: Directory where files will be downloaded
- `file_filter`: Optional file extension filter (default: 'vcf.gz')
- `basespace_access_token`: Your BaseSpace API key

## Usage

1. Set up your BaseSpace API token as described in the Setup Instructions
2. Configure the task parameters in the respective YAML files:
   - Set the project name or biosample name
   - Specify the output directory
   - Optionally set a file filter

## Notes

- The tasks will automatically download the BaseSpace CLI tools
- If multiple projects or biosamples match the specified name, the task will fail
- File filtering is optional - if not specified, all files will be downloaded
- The tasks require minimal resources (1 CPU core, 1GB memory)
