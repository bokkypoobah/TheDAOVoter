# TheDAOVoter

### Description
Perl script to list and vote on The DAO proposals

The script `theDAOVoter` is a small (903 lines, 828 source lines) Perl script that allows you to:
* List The DAO proposals.
* List your accounts, displaying whether The DAO transfers are blocked due to opened votes and expiry time.
* List the DAO proposals with a listing of your accounts showing which accounts have already voted on each proposal. Past votes can also be listed along with the actual gas used.
* Vote on The DAO proposals from your accounts.
* List and sum the votes on split proposals.

This script will run in Linux, should run on Mac OS/X and may run on Windows using one of the Perl distributions including Cygwin and Active State Perl.

<br />

### How Does This Work
This script calls the [Go Ethereum](https://github.com/ethereum/go-ethereum) `geth` program with the `attach` option, running the Go Ethereum JavaScript API to query the Ethereum blockchain. If you want to see the exact commands, add the option `--verbose` to `theDAOVoter`'s command line and all the executions will be revealed.

<br />

### History
* v1.0000000000000000 02/06/2016 First version
* v1.0000000000000001 03/06/2016 Tidy
* v1.0000000000000002 03/06/2016 Added `--checkpastvotes` by retrieving The DAO Voted(...) events
* v1.0000000000000003 04/06/2016 Display account The DAO token blocked status and unblock time
* v1.0000000000000004 05/06/2016
    * `--sumsplits` to list the sum of splits
    * `--account` can now be used to specify an account not in your keystore

<br />

### Sample
Following are some sample uses of this script with results. Add the parameter `--verbose` if you want to see exactly what `theDAOVoter` is doing.

    # List all your accounts including the totals
    user@Kumquat:~$ theDAOVoter --listaccounts
      # Account                                                            ETH                        DAO The DAO transfer blocked by OPEN proposal?
    --- ------------------------------------------ --------------------------- -------------------------- ------------------------------------------
      0 0xaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa      111.111111111111111111       111.0000000000000000 #2 OPEN until Sun Jun 12 03:18:37 2016
      1 0xbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb      222.222222222222222222       222.0000000000000000
    --- ------------------------------------------ --------------------------- -------------------------- ------------------------------------------
      3 Total                                           333.333333333333333333       333.0000000000000000

    # List proposal #2 checking the voting status of this proposal from your accounts
    user@Kumquat:~$ theDAOVoter --listproposals --id=2 --checkvotingstatus --checkpastvotes
    =========================================================================================================================================
    Proposal 2. OPEN until Sun Jun 12 03:18:37 2016
    Votes       Yea 2473115 (44.20%) Nay 3122385 (55.80%) Quorum 0.48% of 20%
    Creator     0x5a8e70f2d75c1468db4a2241fdd70e5a84f028b8
    Recipient   0xbb9bc244d798123fde783fcc1c72d3bb8c189413
    Deposit     2 ETH
    Amount      0 ETH
    New curator N
    -----------------------------------------------------------------------------------------------------------------------------------------
    Do you believe in god?
    -----------------------------------------------------------------------------------------------------------------------------------------

      # Account                                                            ETH                        DAO  Est Gas Voting Status
    --- ------------------------------------------ --------------------------- -------------------------- -------- -------------
      0 0xaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa      111.111111111111111111       111.0000000000000000    56287 Voted Nay
      1 0xbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb      222.222222222222222222       222.0000000000000000    70851 Not voted yet
    --- ------------------------------------------ --------------------------- -------------------------- -------- -------------
    =========================================================================================================================================

    # A NO vote on proposal #2 from account #1
    user@Kumquat:~$ theDAOVoter --vote --id=2 --account=1 --support=0
    Enter password for 0xbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb to vote: 
    Transaction Id 0x5555555555555555555555555555555555555555555555555555555555555555
    
    # Check total The DOA proposals splitting
    user@Kumquat:~$ theDAOVoter --sumsplits
      Prop             Yea             Nay Address                                    Open   Description                             
    ------ --------------- --------------- ------------------------------------------ ------ ----------------------------------------
         1       967598.22      4276278.60 0x13680fa2a60fd551894199f009cca20fb63a3e31 OPEN                                           
         4         5279.34      4321941.58 0x3d5507b53d1613d8491a606ecf5c9268301095dd OPEN   split                                   
         6            1.99       146492.30 0xbeb0b93c01297146782a5581370489a36b24deca OPEN   Original intent, non-interventionist cur
        44        25000.00        44306.95 0x5a422fb07fc9270f5b310fc61f85b8e779cb29a2 OPEN   Hotdog cheap plot gongzho dao           
    ------ --------------- --------------- ------------------------------------------ ------ ----------------------------------------
    SubTot            0.00            0.00                                            CLOSED
    SubTot      5675420.80     87138029.50                                            OPEN
    Total       5675420.80     87138029.50                                            Both
    ------ --------------- --------------- ------------------------------------------ ------ ----------------------------------------


<br />

### Installation And Execution
Download `theDAOVoter` into `$HOME/bin/theDAOVoter` and set the executable bit using

    chmod 700 $HOME/bin/theDAOVoter
    
Before running this script, start the Go Ethereum `geth` node client using the command:

    geth console
    
If you are running the 64-bit Ethereum Wallet (Mist), you will find the `geth` executable in your Ethereum Wallet installation directory under the subdirectory `resources/node/geth/geth`. Change `$GETHBINARY` in the `theDAOVoter` script from

    my $GETHBINARY = "geth";

to

    my $GETHBINARY = "{full geth executable file name including path}";
    
Run the script without any parameters to view the following help text:

    user@Kumquat:~$ theDAOVoter
    
    The DAO Voter v1.0000000000000003 03/06/2016. https://github.com/BokkyPooBah/TheDAOVoter
    
    Usage: theDAOVoter {command} [options]
    
    Commands are:
      --listaccounts
      --listproposals
      --sumsplits
      --vote
      --help
    
    The --listaccounts command has no additional optional parameters other than the general 
    parameters listed below.
    
    The --listproposals command has additional optional parameters:
      --id={proposal id}             Proposal id.
      --first={first proposal id}    First proposal id. Default '1'.
      --last={last proposal id}      Last proposal id. Default last proposal id.
      --split={exclude|include|only} Include splits. Default 'exclude'.
      --status={open|closed|both}    Proposal status. Default 'open'.
      --checkvotingstatus            Check your voting status for the proposals. Default off. This
                                     check uses eth.estimateGas() API call to determine if you have
                                     already voted.
      --checkpastvotes               Retrieve your past voting history. Default off. Actual gas used
                                     will be reported in the (Est)Gas column
    
    The --sumsplits command has no additional option.
    
    The --vote command has the additional options:
      --id={proposal id}             Proposal id.
      --support={0|n|1|y}            Don't support (0 or n) or support (1 or y) proposal.
      --force                        Force a vote even when this tool reports that you have already voted.
    
    There following options can be use generally:
      --account={account or id}      Vote from account number (e.g. 1) or address (e.g. 0xabc...)
      --verbose                      Display what this script is doing.
    
    HISTORY
      v1.0000000000000000 02/06/2016 First version
      v1.0000000000000001 03/06/2016 Tidy
      v1.0000000000000002 03/06/2016 Added --checkpastvotes by retrieving The DAO Voted(...) events
      v1.0000000000000003 04/06/2016 Display account The DAO token blocked status and unblock time
      v1.0000000000000003 05/06/2016 --sumsplits to list the sum of splits
                                     --account can now be used to specify an account not in your keystore
    
    
    REQUIREMENTS - This script runs on Linux and perhaps OSX. You can try it with Cygwin Perl, Strawberry Perl
    or ActiveState Perl on Windows. 
    
    You will need to have geth (go Ethereum node software) on your path. geth is available from 
    https://github.com/ethereum/go-ethereum/releases .
    
    geth is also packaged with the Ethereum Wallet (Mist) downloads. You will find the geth binary in the Ethereum
    Wallet subdirectory under resources/node/geth/geth . Modify the GETHBINARY variable above to point to your geth path
    if necessary.
    
    
    NOTE - Only the --vote command will require you to enter your password to unlock your geth keystore. Check the code
    below if you are concerned. The other --listaccounts and --listproposals commands do not require the unlocking of
    your geth keystore.
    
    
    WARNING - This script uses the same method as the Ethereum Wallet (Mist) to unlock your account in geth
    when you are sending your vote to the Ethereum blockchain. Make sure that you start geth without the 
    --rpc option when using geth with this script. See the following URL about the security issues with this keystore
    unlocking methodology:
    http://ethereum.stackexchange.com/questions/3887/how-to-reduce-the-chances-of-your-ethereum-wallet-getting-hacked
    
    
    The more frequently used commands follow:
      This help
        theDAOVoter
      List accounts and display whether the account is blocked by votes in progress
        theDAOVoter --listaccounts
      List proposals (excluding splits, open proposals only)
        theDAOVoter --listproposals 
      List proposals (excluding splits, open proposals only) and check voting status for your accounts
        theDAOVoter --listproposals --checkvotingstatus
      List proposals #2 and check voting status for your accounts
        theDAOVoter --listproposals --id=2 --checkvotingstatus
      List open proposals and check voting status and past votes for your accounts
        theDAOVoter --listproposals --checkvotingstatus --checkpastvotes
      View split proposal statistics
        theDAOVoter --sumsplits
      Vote on proposal #2 from account #1 in your keystore, not supporting this vote
        theDAOVoter --vote --id=2 --account=1 --support=0
      Vote on proposal #43 from account 0xaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa, supporting this vote
        theDAOVoter --vote --id=43 --account=0xaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa --support=1
    
    
    Donations happily accepted to Ethereum account 0xbeef281b81d383336aca8b2b067a526227638087.
    
    Enjoy, and vote well. BokkyPooBah 2016.
    
    Stopped at theDAOVoter line 266.
        
<br />

### The More Frequently Used Commands
Help

    theDAOVoter

List accounts and display whether the account is blocked by votes in progress

    theDAOVoter --listaccounts

List proposals (excluding splits, open proposals only)

    theDAOVoter --listproposals 

List proposals (excluding splits, open proposals only) and check voting status for your accounts

    theDAOVoter --listproposals --checkvotingstatus

List proposals #2 and check voting status for your accounts

    theDAOVoter --listproposals --id=2 --checkvotingstatus

List open proposals and check voting status and past votes for your accounts

    theDAOVoter --listproposals --checkvotingstatus --checkpastvotes

Vote on proposal #2 from account #1, not supporting this vote

    theDAOVoter --vote --id=2 --account=1 --support=0

<br />

### Go Ethereum (`geth`) JavaScript API Commands Used And TheDAO Functions Called

**Listing Balance**
* eth.getBalance(account)
* theDAO.balanceOf(account)
* theDAO.blocked(account)
* theDAO.proposals(proposalId)

**Listing Proposals**
* theDAO.numberOfProposals()
* theDAO.proposals(proposalId)
* theDAO.minQuorumDivisor()
* theDAO.totalSupply()

**Check Voting Status**
* eth.estimateGas(theDAO.vote(...))

**Check Voting History**
* theDAO.Voted.watch(...)
* eth.getTransactionReceipt(...) 

**Voting**
* personal.unlockAccount(...)
* theDAO.vote(...)

**References**
* [Ethereum - Web3 JavaScript Ðapp API](https://github.com/ethereum/wiki/wiki/JavaScript-API)
* [EtherScan.io - The DAO Source Code](http://etherscan.io/address/0xbb9bc244d798123fde783fcc1c72d3bb8c189413#code)

<br />

### Licence
The MIT License (MIT)

Copyright (c) 2016 bokkypoobah

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

<br />

### And If You Like This
Donations happily accepted to Ethereum account 0xbeef281b81d383336aca8b2b067a526227638087.


Enjoy, and vote well. BokkyPooBah 2016.
