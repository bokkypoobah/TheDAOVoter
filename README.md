# TheDAOVoter

### Description
Perl script to list and vote on The DAO proposals

The script `theDAOVoter` is a small (1,000 lines, 911 source lines) Perl script that allows you to:
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
* v1.0000000000000005 05/06/2016 `--decimalplaces` 
* v1.0000000000000006 06/06/2016 Improved error handling, displaying durations with time
* v1.0000000000000007 07/06/2016 Tidy yea/YEA/nay/NAY
* v1.0000000000000008 07/06/2016 --id renamed to --proposalid, improved --sumsplits
* v1.0000000000000009 07/06/2016 --sumsplits report statistics

<br />

### Sample
Following are some sample uses of this script with results. Add the parameter `--verbose` if you want to see exactly what `theDAOVoter` is doing.

    # List all your accounts including the totals
    user@Kumquat:~$ theDAOVoter --listaccounts --decimalplaces=2
      # Account                                            ETH          DAO The DAO transfer blocked by OPEN proposal?
    --- ------------------------------------------ ----------- ------------ ------------------------------------------
      0 0xaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa      111.11       111.00 #2 OPEN until Sun Jun 12 03:18:37 2016
      1 0xbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb      222.22       222.00
    --- ------------------------------------------ ----------- ------------ ------------------------------------------
      3 Total                                           333.33       333.00

    # List proposal #2 checking the voting status of this proposal from your accounts
    user@Kumquat:~$ theDAOVoter --listproposals --id=2 --checkvotingstatus --checkpastvotes --decimalplaces=2
    ==============================================================================================
    Proposal 2. OPEN until Sun Jun 12 03:18:37 2016
    Votes       Yea 2473115 (44.20%) Nay 3122385 (55.80%) Quorum 0.48% of 20%
    Creator     0x5a8e70f2d75c1468db4a2241fdd70e5a84f028b8
    Recipient   0xbb9bc244d798123fde783fcc1c72d3bb8c189413
    Deposit     2 ETH
    Amount      0 ETH
    New curator N
    ----------------------------------------------------------------------------------------------
    Do you believe in god?
    ----------------------------------------------------------------------------------------------

      # Account                                            ETH          DAO (Est)Gas Voting Status
    --- ------------------------------------------ ----------- ------------ -------- -------------
      0 0xaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa      111.11       111.00    56287 Voted Nay
      1 0xbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb      222.22       222.00    70851 Not voted yet
    --- ------------------------------------------ ----------- ------------ -------- -------------
    ==============================================================================================

    # A NO vote on proposal #2 from account #1
    user@Kumquat:~$ theDAOVoter --vote --id=2 --account=1 --support=0
    Enter password for 0xbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb to vote: 
    Transaction Id 0x5555555555555555555555555555555555555555555555555555555555555555
    
    # Check total The DOA proposals splitting
    user@Kumquat:~$ theDAOVoter --sumsplits
       Prop      Open             Yea             Nay Recipient                                  Description                             
    ------- --------- --------------- --------------- ------------------------------------------ ----------------------------------------
          1 Expired         967598.22      4276278.60 0x13680fa2a60fd551894199f009cca20fb63a3e31                                         
          4 Expired           5279.34      4322941.58 0x3d5507b53d1613d8491a606ecf5c9268301095dd split                                   
          6 Open                 1.99       169759.64 0xbeb0b93c01297146782a5581370489a36b24deca Original intent, non-interventionist cur
          7 Expired         118006.68      3967413.62 0xe82d5b10ad98d34df448b07a5a62c1affbef758f Leave me alone                          
          8 Expired         199999.99      3931880.95 0xa72ded5c1122312d9f4ed66bf4a396139eadaf56                                         
          9 Expired        2659899.77      3911880.49 0x228d29ea776cb17ca0db0538562ecaacdc0a9e46                                         
         10 Expired          15746.04      3933147.83 0x374139a05ac55917badd3f934f1b93f5c8623ded                                         
         12 Expired           5281.35      3945155.27 0xcdc00dd1459e293c9c81880a2b3c4e5396a8ef7b �� cdc00 split proposal ��      
         13 Expired          46039.25      3926435.27 0xf8f9fc62a19c87c657a06febd184f068c0fc9cae arbitrage ftw                           
         14 Expired          51751.22      3873726.70 0x1502447aadf5979e7a842709cd6c4f60afb0a281                                         
         16 Expired         130199.49      3892661.67 0x7c81d252d9d1295058cd3620835f37e0eedd8840 Split 0x7C8                             
         18 Expired           2200.20      3913649.01 0x13680fa2a60fd551894199f009cca20fb63a3e31                                         
         19 Expired        1686495.65      3906900.95 0xf398c9b8107dccc697546969fb2d5956762b60fb split-ID-x8nj2z                         
         20 Expired           1000.00      3906620.95 0xe7535ddfcbefe5c318d271476d068d5f7cf77290                                         
         21 Expired              0.00      3866620.95 0xe7535ddfcbefe5c318d271476d068d5f7cf77290                                         
         22 Expired         239999.99      3894155.12 0x95a61f934d66580dd410a7369f9c5b8e228d2ff3                                         
         23 Open              9999.85      3885353.61 0x357d083321319cc1a8ebad90ba1db06c8698eef6 sploot                                  
         24 Open                10.00      3885353.61 0x3065a8444787f076bff10e5df3ec66606e3c8b68 WL split                                
         25 Open            100000.00      3885353.61 0x1873f651ecf56d27c01d8d17a1bf06a9acf8830b 0x187 canonball                         
         26 Open             10000.00      3885353.61 0x2b15c5211bda6a867c582080536f6c61766aa5af 0x2b15 DAO Split                        
         27 Open              4117.33      3881046.67 0xa7c605a1aacb641d873c82f9b2715e87339dfd48 n0k0 split                              
         28 Open             22737.70      3885353.61 0xb18e6467db64686dfed14c7368ca59e5019c95c8 0xB18e split                            
         29 Open           1275842.51        46973.61 0xd68ba7734753e2ee54103116323aba2d94c78dc5                                         
         30 Open             24653.70        44306.95 0x479abf2da4d58716fd973a0d13a75f530150260a                                         
         31 Open              7318.67        44306.95 0xf8c3879ee8dde81f074abca79b2270eab9942ec1 my_pitui                                
         32 Open             18007.67        44306.95 0xb42da5b3701a0592e5aa0aebc0c20711bd49fb46 0xB42 private split                     
         33 Open              7312.66        44306.95 0xcf69ab35bb6a87a68ce83571a174eef4f998baa7                                         
         34 Open              5000.00        45306.95 0xfdf97eaa34a883647fac329926b1747e9ef601c6 arbitrageservice 0.00 - test            
         35 Open               331.23        45306.95 0xaf496a1083a3a7c7edb831f2e9a31eb065f5a228 E's Castle Rock                         
         36 Open                 9.00        45306.95 0xaf496a1083a3a7c7edb831f2e9a31eb065f5a228 Galt's Gulch                            
         37 Open              7676.17        44306.95 0x98dac39fdcc5c9a8dfc6f63898b62704806851b4 0x98dac split                           
         38 Open              4542.00         4306.95 0x3822e5ff792e75817b89f5bbb405fc4a9d1a0552 The Ilium Works                         
         39 Open                 0.00      3904410.23 0xf4c0eef475ab35625ac223394f9c410ccb577747 GFX, others please don't vote           
         41 Open                 0.19        50203.17 0xfaed3f06255794bf3f83d7ab08d4554d5d218b41                                         
         42 Open             54977.28        50203.17 0xbb9bc244d798123fde783fcc1c72d3bb8c189413                                         
         44 Open             25000.00        71740.72 0x5a422fb07fc9270f5b310fc61f85b8e779cb29a2 Hotdog cheap plot gongzho dao           
         45 Open                99.99         5896.22 0x5824a7486ea2ec17749f936c7b89faa4972f8eb1                                         
         46 Open                99.99         5896.22 0x5824a7486ea2ec17749f936c7b89faa4972f8eb1                                         
         47 Open                99.99         5896.22 0x5824a7486ea2ec17749f936c7b89faa4972f8eb1                                         
         48 Open                99.99         5896.22 0x5824a7486ea2ec17749f936c7b89faa4972f8eb1                                         
         49 Open               737.68         5896.22 0x5824a7486ea2ec17749f936c7b89faa4972f8eb1                                         
         50 Open                 0.00         5896.22 0x0ee82c5e35cbcf9e1271808f0386b930ee8ae8a2 m split                                 
         52 Open               300.00         5896.22 0x56ae819a1bc418121a6e8428b5884f7604152322 test split 1337                         
         53 Open                 0.00         5896.22 0x4853143d0f5524df67a0a5bdd2fb63c76c7693f6 arbitrage ftw 2                         
         54 Open                 0.00            0.00 0x0f935781046701897c9e0d9876fb5c82d89d53be split me baby one more time             
         55 Open                 0.00            0.00 0xf2a83b593162d77c62337a02668be1ee088cb55d 0xF2a83 Split                           
    ------- --------- --------------- --------------- ------------------------------------------ ----------------------------------------
    Closed                       0.00            0.00 Yeas 0.00% of the original supply
    Expired                6129497.19     59469468.96 Yeas 0.52% of the original supply
    Open                   1578975.59     28010037.57 Yeas 0.13% of the original supply
    Total                  7708472.78     87479506.53 Yeas 0.66% of the original supply
    Supply  Current     1167249986.79                 Reduction from original supply of 5269084.68 or 0.45%
    Supply  Original    1172519071.47
    ------- --------- --------------- --------------- ------------------------------------------ ----------------------------------------
    Notes: Nay votes don't affect split votes
    Generated by theDAOVoter at Tue Jun  7 21:37:04 2016
        
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
    
    The DAO Voter v1.0000000000000009 07/06/2016. https://github.com/BokkyPooBah/TheDAOVoter
    
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
      --proposalid={id}                Proposal id.
      --first={first proposal id}      First proposal id. Default '1'.
      --last={last proposal id}        Last proposal id. Default last proposal id.
      --split={exclude|include|only}   Include splits. Default 'exclude'.
      --status={open|closed|both}      Proposal status. Default 'open'.
      --checkvotingstatus              Check your voting status for the proposals. Default off. This
                                       check uses eth.estimateGas() API call to determine if you have
                                       already voted.
      --checkpastvotes                 Retrieve your past voting history. Default off. Actual gas used
                                       will be reported in the (Est)Gas column
    
    The --sumsplits command has no additional option.
    
    The --vote command has the additional options:
      --account={account or id}        Use account number (e.g. 1) or address (e.g. 0xabc...)
      --proposalid={id}                Proposal id.
      --support={0|n|1|y}              Don't support (0 or n) or support (1 or y) proposal.
      --force                          Force a vote even when this tool reports that you have already voted.
    
    There following options can be use generally:
      --account={account or id}        Use account number (e.g. 1) or address (e.g. 0xabc...)
      --decimalplaces={decimal places) Number of decimal places. Default ETH '18', DAO '16'.
      --verbose                        Display what this script is doing.
    
    HISTORY
      v1.0000000000000000 02/06/2016 First version
      v1.0000000000000001 03/06/2016 Tidy
      v1.0000000000000002 03/06/2016 Added --checkpastvotes by retrieving The DAO Voted(...) events
      v1.0000000000000003 04/06/2016 Display account The DAO token blocked status and unblock time
      v1.0000000000000004 05/06/2016 --sumsplits to list the sum of splits
                                     --account can now be used to specify an account not in your keystore
      v1.0000000000000005 05/06/2016 --decimalplaces
      v1.0000000000000006 06/06/2016 Improved error handling, displaying durations with time
      v1.0000000000000007 07/06/2016 Tidy yea/YEA/nay/NAY
      v1.0000000000000008 07/06/2016 --id renamed to --proposalid, improved --sumsplits
      v1.0000000000000009 07/06/2016 --sumsplits report statistics
    
    
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
    
    Stopped at theDAOVoter line 283.
    
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
<pre>
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
</pre>

<br />

### And If You Like This
Donations happily accepted to Ethereum account 0xbeef281b81d383336aca8b2b067a526227638087.


Enjoy, and vote well. BokkyPooBah 2016.
