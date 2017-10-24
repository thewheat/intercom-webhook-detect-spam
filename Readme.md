# Intercom Webhook Process Spam
- This is [Intercom webhook](https://docs.intercom.io/integrations/webhooks) processing code to 
   - reassign/close conversations to a custom admin/inbox when it is detected as spam
   - Reassignment/Closure is done via the [Intercom API](https://developers.intercom.io/reference)
- For emails not caught by [Intercom's spam filter](https://docs.intercom.com/faqs-and-troubleshooting/your-team-inbox/how-do-i-block-spam-in-my-team-inbox) this allows you to have some custom processing to detect spam and reassign it to an inbox or just close it based on your configuration
- Custom processing currently involves looking for specific text phrases in a new incoming message. Looks at both subject (for emails) and body of the message

## Setup - Environment Variable Configuration
- lists all variables needed for this script to work
- `TOKEN`
	- Requires an extended access token to read conversation to identify if it is from the help center
	- Apply for an access token  https://app.intercom.io/developers/_
	- Read more about access tokens https://developers.intercom.com/reference#personal-access-tokens-1 
- `bot_admin_id`
	- the ID of the admin that performs the reassignment (must be an admin not a team)
	- retrieve IDs via Admin API https://developers.intercom.io/reference#admins
- `spam_action`
	- set to `close` if you wish to close converstaions that are detected as spam
	- any value that is not `close` will reassign conversations
- `assignee_admin_id`
	- the ID of the teammate or inbox to reassign conversations to 
- `text_matches`
	- JSON stringify array of phrases to look for in the message
	- Any phrase found will mark conversation as spam
- For development just rename `.env.sample` to `.env` and modify values appropriately

## Running this locally

```
gem install bundler # install bundler
bundle install      # install dependencies
ruby app.rb         # run the code
ngrok http 4567     # uses https://ngrok.com/ to give you a public URL to your local code to process the webhooks
```

- Create a new webhook in the [Intercom Developer Hub](https://app.intercom.io/developers/_) > Webhooks page
- Listen on the following notification: "New message from a user or lead" / `conversation.user.created`
- In webhook URL specify the ngrok URL

