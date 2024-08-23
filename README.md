# I-deployed
# This is the first deployment
# First deployment transaction ID
at1zuf98an9ste5ddz8kcpwljz5cgk36zrfhvvw5rm566epgx598s9q5fagra

# Details
Using the leo run main 3u32 2u32 command i was able to create my main.aleo file in the playgroud where i got my code that i pasted on my demo.leo.app/deploy to succesfully deploy this program
https://photos.google.com/photo/AF1QipPjomWKgUfYot13iE4NdZDfa2-cfwVwHsZDArGC
# Second
# Second deploy transaction ID
at1p2hucd33kywvyjy542wmlqaft6smfqd4rnd2e82jq98jnqdfy5rs8zsw8g

# Details
Using my Playground i ran this command leo run main 3u32 2u32 to build a main.aleo file inside the simple token then i copied the code in the main.aleo and used it to deploy on  my demo.leo.app/deploy
https://photos.google.com/photo/AF1QipOZ6M1BADuHTpP5vQluOO238iHxbWMArwlvqkZ5
# Third
# Third deployment Transaction ID 
at1znw0504423kfvke2s5zuyrx6qjwah4et0jenr63lnetsjze9y5xq3yhj7e

# Details
Using this 
// The 'aleo_voice7' program.
program aleo_voice7.aleo {

    //Record for Voice data
    record Voice {
        owner: address,
        receiver: address,
        msg: u128,
        hash_msg: u128,
    }

    //Record for combining both owner and receiver
    record CoBind {
        hash_owner: field,
        owner: address, 
        receiver: address,
        bind_hash: field,
    }

    // struct data for storing user data
    struct Messaging {
        messenger: field,
        total_messages: u64,
    }

    //Mapping for  user data
    mapping voice_msg: field => Messaging; 

    //send voice transition function with the async await js to update state. has 4 inputs
    async transition send_voice (owner: address, receiver: address, msg: u128,  co_bind: field) -> (Voice, Future){
        //hash a message
        let hash: u128 = BHP256:: hash_to_u128(msg);

        //checks if address calling match owner address
        assert(self.caller == owner);

        //checks address calling is not a receiver
        assert_neq(self.caller, receiver);
        
        //hash the owner
        let hash_owner: field = BHP256:: hash_to_field(owner);

        //hash the receiver
        let hash_receiver: field = BHP256:: hash_to_field(receiver);

        //add both [owner and receiver] hash
        let add_hash : field = hash_owner + hash_receiver ;

        // declaring them into a record -> Voice
        let send_from : Voice = Voice {
            owner: owner,
            receiver: receiver,
            msg: msg,
            hash_msg: hash
        };
        
        //return the record and update the state onchain with the function name - finalize_send_voice
        return (send_from, finalize_send_voice(owner, hash));

    }
    //state being updated
    async function finalize_send_voice (owner: address, hash: u128){
        //hash the owner
        let hash_owner: field = BHP256:: hash_to_field(owner);
        // getting the mapping detail, by default mapping is to -> hash_owner  and 0 total messages
        let total: Messaging = Mapping::get_or_use(voice_msg, hash_owner, Messaging{
            messenger: hash_owner,
            total_messages: 0u64,
        });
        
        //storing/setting the data into the mapping, now the data component will be uopdate to the hash_owner and n total messages
        voice_msg.set(hash_owner, Messaging {
            messenger: hash_owner,
            total_messages: total.total_messages + 1u64,
        });
    }

    transition combine_hash_owner_receiver(owner: address, receiver: address) -> (CoBind){
        assert_eq(owner, self.caller);
        let hash_owner: field = BHP256::hash_to_field(owner);
        let hash_receiver: field = BHP256::hash_to_field(receiver);
        let add_hash: field = hash_owner + hash_receiver;

        let update_hash: CoBind = CoBind {
            hash_owner: hash_owner,
            receiver: receiver,
            owner: owner,
            bind_hash: add_hash,
        };
        return (update_hash);
    }
} 
I was able to build my main.aleo from the simple token file, then i copied the code the, changed the private key to my own private key in the environment and then entered the terminal in the playground then i ran this code format > leo run

combine_hash_owner_recei ver <type_your_address>

<type_friend_address> deployed successfuly in my demo.leo.app/deploy
https://photos.google.com/photo/AF1QipMEs1HsiaH6UmXEOvwv0UjVnq5fqHJmiUhfO7Fv
https://photos.google.com/photo/AF1QipNwXYZgoiKz-jmDM-ovFH252kh9EEpGzgGrDp--
