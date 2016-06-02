# TheDAOVoter
Perl script to list and vote on The DAO proposals

The script `theDAOVoter` is a small (~620 lines) Perl script that allows you to list The DAO proposals, list your accounts, and vote on The DAO proposals from your accounts. `theDAOVoter` will also list The DAO proposals and your accounts with the voting status on each proposal from your accounts.

This script will run in Linux, should run on Mac OS/X and may run on Windows using one of the Perl distributions including Cygwin and Active State Perl.

Before running this script, start the Go Ethereum node client using the command:

    geth console

You can then run `$HOME/bin/theDAOVoter`. Following are some sample uses of this script with results. Add the parameter `--verbose` if you want to see exactly what `theDAOVoter` is doing.

    # List all your accounts including the totals
    user@Kumquat:~$ theDAOVoter --listaccounts
      # Account                                                            ETH                        DAO
    --- ------------------------------------------ --------------------------- --------------------------
      0 0xaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa      111.111111111111111111       111.0000000000000000
      1 0xbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb      222.222222222222222222       222.0000000000000000
    --- ------------------------------------------ --------------------------- --------------------------
      3 Total                                           333.333333333333333333       333.0000000000000000

    # List proposal #2 checking the voting status of this proposal from your accounts
    user@Kumquat:~$ theDAOVoter --listproposals --id=2 --checkvotingstatus
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
      0 0xaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa      111.111111111111111111       111.0000000000000000  1000000 Already voted
      1 0xbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb      222.222222222222222222       222.0000000000000000    70851 Not voted yet
    --- ------------------------------------------ --------------------------- -------------------------- -------- -------------
    =========================================================================================================================================

    # A NO vote on proposal #2 from account #1
    user@Kumquat:~$ theDAOVoter --vote --id=2 --account=1 --support=0
    Enter password for 0xbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb to vote: 
    Transaction Id 0x5555555555555555555555555555555555555555555555555555555555555555

Download `theDAOVoter` into `$HOME/bin/theDAOVoter` and set the executable bit using

    chmod 700 $HOME/bin/theDAOVoter
    
Run the script without any parameters to view the following help text:

    user@Kumquat:~$ theDAOVoter
    
    The DAO Voter v 1.0000000000000001 02/06/2016.
    
    Usage: /home/user/bin/theDAOVoter {command} [options]
    
    Commands are:
      --listaccounts
      --listproposals
      --vote
      --help
    
    The --listaccounts command has no additional option.
    
    The --listproposals command has additional optional options:
      --id={proposal id}             Proposal id.
      --first={first proposal id}    First proposal id. Default '1'.
      --last={last proposal id}      Last proposal id. Default last proposal id.
      --split={exclude|include|only} Include splits. Default 'exclude'.
      --status={open|closed|both}    Proposal status. Default 'open'.
      --checkvotingstatus            Check your voting status for the proposals. Default off.
    
    The --vote command has the additional options:
      --id={proposal id}             Proposal id.
      --account={account or id}      Vote from account number (e.g. 1) or address (e.g. 0xabc...).
      --support={0|n|1|y}            Don't support (0 or n) or support (1 or y) proposal.
      --force                        Force a vote even when this tool reports that you have already voted.
    
    There following options can be use generally:
      --verbose                      Display what this script is doing.
    
    The following commands are the more frequently used ones:
      This help
        /home/user/bin/theDAOVoter
      List accounts
        /home/user/bin/theDAOVoter --listaccounts
      List proposals (excluding splits, open proposals only)
        /home/user/bin/theDAOVoter --listproposals 
      List proposals (excluding splits, open proposals only) and check voting status for your accounts
        /home/user/bin/theDAOVoter --listproposals --checkvotingstatus
      List proposals #2 and check voting status for your accounts
        /home/user/bin/theDAOVoter --listproposals --id=2 --checkvotingstatus
      Vote on proposal #2 from account #1, not supporting this vote
        /home/user/bin/theDAOVoter --vote --id=2 --account=1 --support=0
    
    
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
    
    
    Donations happily accepted to Ethereum account 0xbeef281b81d383336aca8b2b067a526227638087.
    
    Enjoy, and vote well. BokkyPooBah 2016.
    
    Stopped at /home/user/bin/theDAOVoter line 244.
    
