# AAVE-Delta-Neutral-st-yCRV Strategy for Yearn V3
Strategy collateralizes asset on AAVE (V3) and borrows CRV token to deposit into st-yCRV.

## How to run locally

### Requirements
    Python >=3.8.0, <=3.10
    Yarn
    Node.js >=14
    Hardhat

### Fork this repository

    git clone https://github.com/user/tokenized-strategy-ape-mix

    cd tokenized-strategy-ape-mix

### Set up your virtual environment

    python3 -m venv venv

    source venv/bin/activate

Tip: You can make them persistent by adding the variables in ~/.env (ENVVAR=... format), then adding the following in .bashrc: `set -a; source "$HOME/.env"; set +a`

### Install Ape and all dependencies

    pip install -r requirements.txt
    
    yarn
    
    ape plugins install .
    
    ape compile
    
    ape test
    
### Set your environment Variables

    export WEB3_INFURA_PROJECT_ID=your_infura_api_key

    export ETHERSCAN_API_KEY=your_api_key



## Testing

Due to the nature of the BaseTokenizedStrategy utilizing an external contract for the majority of its logic, the default interface for any tokenized strategy will not allow proper testing of all functions. Testing of your Strategy should utilize the pre-built `IStrategyInterface` interface to cast any deployed strategy through for testing, as seen in the confest example. You can add any external functions that you add for your specific implementation to this interface to be able to test all functions with one variable. 

Example:

    strategy = management.deploy(project.Strategy, asset, name)
    strategy =  project.IStrategyInterface.at(strategy.address)

Due to the permissionless nature of the tokenized Strategies, all tests are written without integration with any meta vault funding it. While those tests can be added, all V3 vaults utilize the ERC-4626 standard for deposit/withdraw and accounting, so they can be plugged in easily to any number of different vaults with the same `asset.`

#### Errors:

"DecodingError: Output corrupted.": Probably due to not running on a forked chain or a chain where the TokenizedStrategy contract isn't deployed.
"No conversion registered to handle": Check the `IStrategyInterface` interface the tests are using is up to date.

### Deployment

#### Contract Verification

Once the Strategy is fully deployed and verified, you will need to verify the TokenizedStrategy functions. To do this, navigate to the /#code page on etherscan.

1. Click on the `More Options` drop-down menu. 
2. Click "is this a proxy?".
3. Click the "Verify" button.
4. Click "Save". 

This should add all of the external `TokenizedStrategy` functions to the contract interface on Etherscan.

See the ApeWorx [documentation](https://docs.apeworx.io/ape/stable/) and [github](https://github.com/ApeWorX/ape) for more information.
