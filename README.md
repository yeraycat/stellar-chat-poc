# stellar-chat-poc
Proof of concept for 100% onchain chat for stellar using manage data operations

## Get started

Change chatroomId to an account you control.

In this account, add manually a data entry for each user you want to add with anything as value.

Then just run the site in a http server or push it to a hosting.



## How it works

- User sends 1 XLM to chatroom account
- Chatroom account uses XLM received to launch a manage_data operation to add the user as chatroom member. This is done by adding a data entry to the account with the user public key as key and the username or any other thing as value. (This step isn't coded yet, you'll have to manually add the user to your chatroom account).
- User creates a data entry in their account using the chatroomId (public key) as key for the data entry and then override this data entry with each message they send.
- Frontend reads 200 last operations for each chatroom member and filters them to just find manage_data operations and then to only data operatios with the chatroomId as key.
- The value for each of these manage_data operations retreived is used as message.
- Every message for the users involved within the 200 operations range is collected and ordered by date, then rendered



It is set to run on testnet, but it is quite easy to change this in the code.


## Improvements to be done

- Extend messages length to 6400 characters by using the 100 operations in a single transaction (each 64 characters is an overwrite over the data entry, so each 64 characters there's a fee increase) and then joining them back to build a message.
- Add code for "server". Node reading event stream coming from horizon to add users through manage_data operations to the chatroom when the payment is received.
