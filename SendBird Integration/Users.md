
POST : create-wa-group 
(user1Id, user2Id)
	if user1  and user2 exists:
		create-wa-group (services)

This service will create a wa-group with channel to join in the channelId. Two people will join this.
This generated will be stored in a new collection.
Response : 
			200 -> {sucess: true, object: {user1token, user2token, channelId}}


POST : delete-wa-group

(user1Id, user2Id)
	if user1 and user2 exists:
		remove-wa-group (services)

This service will check call the platform API of sendbird. To remove the channel and the users. Can be removed as well.

		200 -> {success: true}

