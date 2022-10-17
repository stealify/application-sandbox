# application-sandbox
A Application sandbox guide and how to implement it right.

## History and Learnings
- Linux Jails one of the first attempts to solve the issue of running none trusted code on a platform.
- libcontainer
- wasm
- virtual machines
- binary_sandboxing (As any binary is a stack machine you can manipulate the stack of it as it uses Symbols to reference the internal Tasks/Functions that it uses we can replace that symbols inside the binary.

## Design Patterns
- Permission Based Systems
  - The bad part was we did configure them via settings the range of settings did explode it gets overwhelming we move the problem to a diffrent location
- Capability Based Systems
  - Solve the above mentioned issues as we do not need to understand every setting if we configure new software we only need to understand
  what kind of access rights it has and it has only what gets assigned not more not less. 
    - paired with the component architecture and standards for component communication we can crreate fain grained capability based components.
 
 
 ## Pseudo Examples
 This app wants to use a new Browser Window to show something we create a higher level capability providing component that passes its capabilitys 
 to other components that depend on it 
 ```js
 // This component needs it self to have access to the component-manager else it would not be able
 // to import with the component: protocol 
 import { BrowserWindow } from 'component:webplatform';
 // capabilitys are a set of Tasks(Streams) that allows filtering of passed arguments or even do not allow passing any arrguments 
 export const capabilitys = [() => BrowserWindow.new({ id: "allowed-window" }), () => BrowserWindow.get('allowed-window');]
 const [createWindow, getWindow] = capabilitys;
 ```
 
 ```js
 createWindow({ id: "bad-id" }) // will create allowed-window as the capability accepts no input in this example.
 getWindow({ id: "bad-id" }) // will get allowed-window as the capability accepts no input in this example.
 ```
 
 
