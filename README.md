# How to play

Try the game here: https://brilliancebitcoin.github.io/coinflip/index.html

# Notes on its status as a work-in-progress

(1) I know, it's absolutely horrendous in appearance -- maybe someday it will look pretty but I am not a frontend guy

(2) You need to play it with testnet coins, I use this faucet: https://bitcoinfaucet.uo1.net/send.php -- I usually send money from the faucet directly into the addresses the game tells me to send money to, and I usually use the faucet's address as the withdrawal addresses for both players

(3) I store game state and private keys in browser storage and I clear the browser storage every time you reload the page, so don't hit refresh til your game is done

(4) All needed functionality is present. If you follow the procedures outlined in the status messages (and don't hit abort after you've deposited money), the game should work great

(5) I haven't tested what happens if you play the game in two browser sessions that share the same browser storage. It will probably break. So far I always test it with one window in incognito mode and I recommend doing that if you want it to work

(6) Did I say smart contract? Yes, I did. A small program I wrote in Bitcoin Script gets a blinded bit commitment from both users and then runs XNOR on it after the users reveal their bit commitment. If player 2 correctly guessed player 1's bit (1 or 0) then player 2 gets the money in the address, otherwise player 1 gets it. If one party withholds info, the protocol either aborts or that party loses money, depending on circumstances. Also, there is no oracle and no dependency on any third parties except the ordinary trust that all bitcoiners place in the incentives of miners to keep mining and not collude with one another to censor your transactions

(7) Please alert me to any bugs you find
