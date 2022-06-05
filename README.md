# How to play

Try the game here: https://brilliancebitcoin.github.io/coinflip/index.html

# Notes on its status as a work-in-progress

(1) I know, it's absolutely horrendous in appearance -- maybe someday it will look pretty but I am not a frontend guy

(2) Don't trust the abort button. There are parts of the game where it is safe to abort (namely, before you've deposited any money into the smart contract) and there are parts where it is unsafe, but the abort button *always* tells you it is safe. (I plan to work on that.) So don't trust it, but do use it to clear your browser storage before and after each game (I store game state in local storage and session storage, and I haven't tested what happens if you play a new game with old state).

(3) You need to play it with testnet coins, I use this faucet: https://bitcoinfaucet.uo1.net/send.php -- I usually send money from the faucet directly into the addresses the game tells me to send money to, and I usually use the faucet's address as the withdrawal addresses for both players 

(4) Notwithstanding the wonky abort button, I think all needed functionality is present. Meaning, if you follow the procedures outlined in the status messages (and don't hit abort after you've deposited money), the game should work.

(5) I haven't tested what happens if you play the game in two browser sessions that share the same browser storage. It will probably break. So far I always test it with one window in incognito mode and I recommend doing that if you want it to work.

(6) Did I say smart contract? Yes, I did. A small program I wrote in Bitcoin Script gets a blinded bit commitment from both users and then runs XNOR on it after the users reveal their bit commitment. If player 2 correctly guessed player 1's bit (1 or 0) then player 2 gets the money in the address, otherwise player 1 gets it. If one party withholds info, the protocol either aborts or that party loses money, depending on circumstances. Also, there is no oracle and no dependency on any third parties except the ordinary trust that all bitcoiners place in the incentives of miners to keep mining and not collude with one another to censor your transactions.

(7) Please alert me to any bugs you find.
