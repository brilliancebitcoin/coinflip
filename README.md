# How to play

Try the game here: https://brilliancebitcoin.github.io/coinflip/

# How it works

# Remote coinflipping

In 1981, a researcher named Michael Blum invented a remote coinflipping technique. To do it, you pick Heads or Tails and commit to it via a cryptographic hash which you share with your opponent. Then your opponent guesses if you picked Heads or Tails. If you reveal the preimage to your cryptographic hash and it matches your opponent's guess, or if you abort the protocol, it counts as a win for your opponent. If you reveal the preimage to your cryptographic hash and it does not match your opponent's guess, it counts as a win for you. Thus all outcomes after your opponent makes their guess involve either your victory or your defeat with a 50/50 chance either way.

# Blinded bit commitments

It is possible to simulate remote coinflipping on bitcoin's blockchain using bitcoin's built-in programming language -- bitcoin script. Bitcoin script can be used to construct a function called a blinded bit commitment function. A simple blinded bit commitment is a hash of the number 0 or of the number 1 -- one bit. By giving you this hash, your opponent commits to a bit. But you don't know which bit your opponent committed to because you only see a hash of the number they chose, and not the number itself. Thus you are "blind" to what your opponent committed until they reveal it. Once you have your opponent's blinded bit commitment, you are supposed to guess which one they committed -- a 0 or a 1. If you guess correctly, you win the bet amount, otherwise you lose.

In bitcoin we use a slight variation on blinded bit commitments because if you merely ran the number 0 or the number 1 through a hashing function, it would be discernible to your opponent. They could run both numbers through the same hashing function and check which hash matches the one you gave them. To prevent this, you don't commit precisely to the number 0 or the number 1, but to a byte string which is either 16 or 17 bytes in length. Your opponent guesses 16 or 17, then you reveal your bytestring to your opponent, and they check if their guess was right or wrong. This has the same effect as a blinded bit commitment but is more secure for use on the blockchain.

# XOR

One of the functions I need to determine the winner of the coinflip is an XOR logic gate. An XOR logic gate is a fundamental building block of computers which takes two binary digits as input (for example, 0 and 1) and returns a 1 if they are different numbers or a 0 if they are the same number. Bitcoin script has a function called op_numnotequal which I use for this purpose. Op_numnotequal checks whether two numbers are equal. If they are not equal, it returns true, and the program proceeds to the next step (which, in my game, is to check the signature of player 1 and then -- if they supplied a valid signature -- let them withdraw the money). If they are equal, the program returns false, and the transaction fails.

# Timelock

My coinflip game also uses a timelock: once money is deposited into the coinflip address, if 8 blocks go by without player 1 using bytestrings of matching size to withdraw the money, player 2 can withdraw the money. It is also worth noting that player 2 deposits 2x the bet amount into the coinflip address, and player 1 never actually deposits any money into the coinflip address. Instead, player 1 deposits their money into an address that allows player 2 to take it if they reveal their preimage, otherwise player 1 can take their money back out after 4 bitcoin blocks. This technique incentivizes player 2 to reveal his preimage, otherwise he cannot get Alice's deposit and will only get his own money back (thus aborting the game). By taking player 1's money, he must reveal his preimage, which allows player 1 to determine whether she won or not. If she won, she can use her preimage plus player 2's preimage to take player 2's money from the coinflip address, thus getting the full value of her bet back plus an additional 1 the bet amount from her opponent.

Try clicking Heads or Tails below. You can see how your hash will appear on the blockchain, it is displayed as a 40 character string under the coin. Be aware that every time you play the game the hashes will be different because they are not literally a hash of the word Heads or the word Tails. The strings are hashes of random numbers that are either 16 bytes long (Heads) or 17 bytes long (Tails).

When you are ready to commit to your coinflip, click Create. This website will then create an offer and wait for someone to accept it. You'll also get a link you can share if you want to invite someone to accept your coinflip. You won't need to pay money to commit your coinflip to the blockchain til someone accepts your offer. You can also abort at any time except if your opponent wins (which is sort of an auto-abort situation anyway, one where you also can't get your money back).

In this coinflip game, two players make a blinded bit commitment (0 or 1, based on the size of a hash preimage, where they disclose the hash at the beginning of the game) and then player 2 reveals their commitment. Player 1 then runs an xor operation to see if player 2 correctly “guessed” player 1’s bit commitment. If they did, xor returns false, which triggers a transaction failure whereby player 1 cannot withdraw the money. A timelock eventually expires and allows player 2 to withdraw the money. However, if player 2 did not correctly guess player 1’s bit, xor returns true, which triggers a spend path whereby player 1 can withdraw the money. By this mechanism, I can do a fair lottery on bitcoin between two players.

# Notes on its status as a work-in-progress

(1) I know, it's absolutely horrendous in appearance -- maybe someday it will look pretty but I am not a frontend guy

(2) You need to play it with testnet coins, I use this faucet: https://bitcoinfaucet.uo1.net/send.php -- I usually send money from the faucet directly into the addresses the game tells me to send money to, and I usually use the faucet's address as the withdrawal addresses for both players

(3) I store game state and private keys in browser storage and I clear the browser storage every time you reload the page, so don't hit refresh til your game is done

(4) All needed functionality is present. If you follow the procedures outlined in the status messages (and don't hit abort after you've deposited money), the game should work great

(5) I haven't tested what happens if you play the game in two browser sessions that share the same browser storage. It will probably break. So far I always test it with one window in incognito mode and I recommend doing that if you want it to work

(6) Did I say smart contract? Yes, I did. A small program I wrote in Bitcoin Script gets a blinded bit commitment from both users and then runs XNOR on it after the users reveal their bit commitment. If player 2 correctly guessed player 1's bit (1 or 0) then player 2 gets the money in the address, otherwise player 1 gets it. If one party withholds info, the protocol either aborts or that party loses money, depending on circumstances. Also, there is no oracle and no dependency on any third parties except the ordinary trust that all bitcoiners place in the incentives of miners to keep mining and not collude with one another to censor your transactions

(7) Please alert me to any bugs you find
