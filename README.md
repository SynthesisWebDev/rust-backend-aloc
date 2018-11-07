# rust-backend-aloc

The goal: To rewrite the api currently managing data through redux into rust, interfacing with Holochain.

### Rust Docs

Here are the rust docs for the language itself:

https://doc.rust-lang.org/book/2018-edition/ch02-00-guessing-game-tutorial.html

### Installing holochain

How to install holochain rust in your system:

https://github.com/holochain/app-spec-rust/releases

### Guide to implement rust <> Holochain

Here is the guide on how to manage data to/from the DHT:
https://holochain.github.io/holochain-rust/

### Working rust api

A working rust Api set up with holochain:

https://github.com/holochain/app-spec-rust

### Work rust app

A working app example:

https://github.com/holochain/tasktaskic

The general approach:

- start with the tutorial, follow it and make it work, then do it again, but change out for one of the functions of the app, then repeat for the rest of the functions in a reducer, then for the rest of the reducers.

- Alternate between the above, and taking tutorials in rust.

Example reducer to be converted to rust:

```
import {
FETCH_COMMITMENTS,
CREATE_COMMITMENT,
CONFIRM_COMMITMENT,
DELETE_COMMITMENT,
EDIT_COMMITMENT,
ARCHIVE_COMMITMENT,
RESTORE_COMMITMENT,
FETCH_PROFILE,
ACCEPT_ACCOUNTABILITY_BUDDY_INVITATION_FROM_UPDATE,
} from '../actions/actionTypes'

import \_ from 'lodash'

const initialState = {}

export default function(state = initialState, action) {
const { type, payload } = action
switch (type) {
case FETCH*COMMITMENTS:
return *.keyBy(payload, 'id')
case CREATE_COMMITMENT:
return {
...state,
[payload.id]: {
...payload,
confirmed: false,
},
}
case EDIT_COMMITMENT:
return {
...state,
[payload.id]: {
...payload,
},
}
case FETCH_PROFILE:
// TODO wire this up after holochain integration
// this action will grab the specified commitment and add it into application level state
// using axios, the fetch data is available as action.payload.data
// // dont overwrite the previous state
// // but add a new key value pair

      // This will work once your connected to an actual data source
      // return { ...state, [action.payload.data.id]: action.payload.data };

      return state

    case DELETE_COMMITMENT:
      var id = payload.id
      return {
        ...state,
        [id]: {
          ...state[id],
          status: 'deleted',
        },
      }

    case ACCEPT_ACCOUNTABILITY_BUDDY_INVITATION_FROM_UPDATE:
      let {
        id_of_commitment,
        id_of_accountability_buddy,
      } = action.meta.data.commitment_data

      return {
        ...state,
        [id_of_commitment]: {
          ...state[id_of_commitment],
          accountability_buddies: {
            ...state[id_of_commitment].accountability_buddies,
            [id_of_accountability_buddy]: {
              id: id_of_accountability_buddy,
              confirmed: true,
            },
          },
        },
      }

    case CONFIRM_COMMITMENT:
      // pull the id of the commitment off the payload
      var id_of_commitment = payload.id_of_commitment
      // id_of_commitment: 'y6d3ash',
      // id_of_accountability_buddy: '132412341324',
      return {
        // pull the state in
        ...state,
        // BUT, in the case of this commitment, go into it
        [id_of_commitment]: {
          // return its state, except update the props passed
          ...state[id_of_commitment],
          ...payload.meta.data.update_props,
        },
      }
    // set status tag to archived on click
    case ARCHIVE_COMMITMENT:
      var id = payload.id

      return {
        ...state,
        [id]: {
          ...state[id],
          status: 'archived',
        },
      }
    // set status tag to archived on click
    case RESTORE_COMMITMENT:
      var id = payload.id

      return {
        ...state,
        [id]: {
          ...state[id],
          status: 'confirmed',
        },
      }

    default:
      // console.log('default state of commitments reducer fired');
      return state

}
}

//TODO delete this if not used in fetching from the holochain
// export default function(state = initialState, action) {
// switch (action.type) {
// case FETCH*COMMITMENTS:
// console.log(action.payload.data);
// // coerce into an object with id as key
// return *.mapKeys(action.payload.data, 'id');
// default:
// return state;
// }
// }
```
