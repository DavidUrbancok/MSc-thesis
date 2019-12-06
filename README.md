## Folder structure

	.thesis						 - Thesis attachment root folder.
	|- caliper					 - Folder containing all files for benchmarking the networks.
	.	|- benchmark			 - Folder containing the benchmark definition.
	.	.	|- config.yaml       - Performance benchmark configuration file.
	.	.	|- open.js           - Code for account opening.
	.   .   |- query.js          - Code for querying accounts.
	.	.	|- transfer.js       - Code for asset transfer.
	.	|- networks				 - Folder containing all files needed to set up the tested networks.
	.	.	|- ethereum			 - Ethereum blockchain specific files.
	.	.	.	|- ethereum.json - Ethereum network configuration file for the benchmark.
	.	.	.	|- ...			 - Other folders and files for the Ethereum blockchain.
	.	.	|- ...				 - Other tested network definition files.
	.	|- src                   - Folder containing smart contracts that get deployed to the test networks.
	|- results                   - Folder containing complete performance benchmark results for each network.
	.	|- Ethereum				 - Folder containing benchmark results for the Ethereum network. 
	.	.	|- 10 TPS			 - Folder containing the results for 10 TPS send rate.
	.	.	.	|- caliper.log   - Log file containing full output from the benchmark run.
	.	.	.	|- report.html	 - Table view of the benchmark results.
	.	.	|- ...				 - Folder containing the results for other send rates.
	.	|- ...					 - Other tested blockchain results.
	|- README.md				 - Readme file explaining the folder structure, prerequisites, and commands
								   for running the benchmarks on all the tested networks.
-----
## Prerequisites

1. Installed `npm`, `docker`, and `docker-compose`.

		sudo apt-get install npm docker docker-compose
2. Globally installed `npx` package.

		sudo npm install npx -g
3. The current user is a member of the `docker` group.
	
	3.1. Add the `docker` group (if not exists).

		sudo groupadd docker

	3.2. Grant the current user privileges to run *docker* without `sudo`.

		sudo usermod -aG docker $USER

	3.3. Log out and log back in to re-evaluate group memberships or run the following command:

		newgrp docker

	3.4. *(optional)* Verify that *docker* can run without `sudo`.

		docker run hello-world
-----
## Running the benchmarks

1. Using the `terminal`, navigate into the `caliper` folder after unzipping the package.
2. Install NPM packages:

		npm install
3. Bind the desired blockchain to the SDK (use the table below to supply the values for *blockchain* and *version* parameters):

		npx caliper bind --caliper-bind-sut <blockchain> --caliper-bind-sdk <version>

4. Run the benchmark (use the table below to supply the value for *path_to_network* parameter):

		npx caliper benchmark run
		--caliper-workspace .
		--caliper-benchconfig benchmark/config.yaml
		--caliper-networkconfig networks/<path_to_network>

	| blockchain | version | path_to_network          |
	| ---------- | ------- | ------------------------ |
	| fabric     | 1.4.0   | `fabric/fabric.yaml`     |
	| ethereum   | 1.2.1   | `ethereum/ethereum.json` |
	| besu       | 1.3.2   | `besu/besu.json`         |
	| iroha      | 0.6.3   | `iroha/iroha.json`       |

5. The results of the performance benchmarks will be saved to the `caliper` folder in *caliper.log* and *report.html* files.
-----
### Modifying the *Transaction rate*
To modify the transaction rate, navigate to the `caliper/benchmark` folder and edit the `config.yaml` file. The file is annotated with instructions which values to change to modify the *transaction rate* of each of the benchmarking steps: *open*, *query*, and *transfer*.