# Smart Chat

[![npm](https://img.shields.io/npm/v/smart-chat-react.svg?style=for-the-badge)](https://www.npmjs.com/package/smart-chat-react)

A Chat System SDK With On Chain Access Control List.

Built on top of [3Box](https://docs.3box.io/), and enfources smart contract logic for ACL.

### Try the demo [here](https://kickback-app-live-q4d2ow5zo.now.sh/event/0x545a09f0348dd3632809c13ab3b062c7f8542c8d)
![Example Screenshot](https://user-images.githubusercontent.com/46699230/79636618-a83a3880-81ab-11ea-9ba8-4ffed9cc95d8.png)

<br />

### Chat Access Control with Smart Contract

Smart Chat by default uses [Persistent Thread](https://docs.3box.io/build/web-apps/messaging/persistent-threads) to preserve the chat history, and control the permission of members and moderators. The messages in the chat will sustain unless the moderators delete them. By integrating smart contracts into the thread, it enforces on-chain ACL into a Chat System for your DApp, DAO, events, projects and teams.

We'll also extend the support to [Ghost Thread](https://docs.3box.io/build/web-apps/messaging/ghost-threads), and Confidential Thread, and keep the access control with smart contract.



## smart-chat-react

smart-chat-react is a Chat React component, built with the [3box-chatbox](https://github.com/open-tribe/3box-chatbox-react), and support smart contract functions to filter the members / moderators.

## Getting Started

1. Install the component
2. Use the component
3. Chat APIs

### 1. Install the component

```shell
npm i -S smart-chat-react
```


### 2. Use the component

#### Example

```jsx
import ChatRoom from 'smart-chat-react';
const Chat = props => {
  const {
    party,
    web3,
  } = props

  const members = party.participants.map(p => p.user.address)
  const moderators = party.admins.map(p => p.user.address)
  const owner = moderators && moderators.length > 0 ? moderators[0] : members[0];

  let canJoin = null
  let canModerate = null
  if (web3) {
    try {
      const contract = new web3.eth.Contract(Conference.abi, party.address)
      canJoin = {
        contract,
        method: "isRegistered"
      }
      canModerate = {
        contract,
        method: "isAdmin"
      }
    } catch(e) {
      console.log("Failed to load contract", party, e)
    }
  }

  return (
    <ChatRoom
      appName="Kickback"
      channelName={party.address}
      canJoin={canJoin}
      canModerate={canModerate}
      organizer={owner}
      members={members}
      moderators={moderators}
      colorTheme="#6E76FF"
      popup
    />
  )
}
```


#### Prop Types

| Property | Type          | Default  | Required Case          | Description |
| :-------------------------------- | :-------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `appName`    | String        |    |  Always   |  The name of the dApp, which will be used as the 3Box space name by default. |
| `channelName`    | String       |   | Always    | A unique ID/name for this chat |
| `organizer`    | ETH Address         |   | Always   | The organizer of the chat, which will be used as the firstModerator parameter for a [Persistent Thread](https://docs.3box.io/build/web-apps/messaging/persistent-threads) in 3Box |
| `canJoin`    | Object         |   | Always   | The `contract` and `method` for verifying whether an account can join the chat or not |
| `canModerate`    | Object         |   | Always   | The `contract` and `method` to verifying whether an account can join as a moderator/admin of the chat |
| `members`    | Array of ETH Address         |   | Optional   | The members of a chat will be added if provided. |
| `moderators`    | Array of ETH Address         |   | Optional   | The moderators of a chat will be added if provided. |
| `secret`    | Boolean       |  False   | Optional    | A boolean - `true` - to make the chat content only visible to its members. False will make the chat visible to everyone. |
| `colorTheme`    | String/Boolean       |  False  | Optional    | Pass an rgb or hex color string to match the color theme of your application |
| `popup`    | Boolean       |  False   | Optional    | A boolean - `true` - to configure a pop up style chatbox with a button fixed to the bottom right of the window to pop open the chat UI. False will render the component in whichever container you have implemented. |
| `iconUrl`    | String       |    | Optional    | Set the icon for the chat window |

### 3. Chat APIs

Here we support the APIs in both browser and node.js environment, for listing members and moderators of a chat. See the example below.

Let me know if you'd like to add more interfaces such as add members or moderators, which now are only supported via the React components.

#### Example

```js
import { getChat } from 'smart-chat-react';

const chat = await getChat(appName, channelName, organizer)

const members = await chat.listMembers()

const moderaors = await chat.listModerators()

```


## License

MIT



