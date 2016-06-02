# TheDAOVoter
Perl script to list and vote on The DAO proposals

The script `theDAOVoter` is a small (~620 lines) Perl script that allows you to list The DAO proposals, list your accounts, and vote on The DAO proposals from your accounts. `theDAOVoter` will also list The DAO proposals and your accounts with the voting status on each proposal from your accounts.

This script will run in Linux, should run on Mac OS/X and may run on Windows using one of the Perl distributions including Cygwin and Active State Perl.

Here is the sample usage:

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
    user@Kumquat:~$ theDAOCLIVoter --vote --id=2 --account=1 --support=0
    Enter password for 0xbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb to vote: 
    Transaction Id 0x5555555555555555555555555555555555555555555555555555555555555555

Download `theDAOVoter` into `$HOME/bin/theDAOVoter` and set the executable bit using

    chmod 700 $HOME/bin/theDAOVoter
    
Run the script without any parameters to view the help text.
