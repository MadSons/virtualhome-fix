# Document for the rules of the transition

## Preliminaries

### Possible relations

Possible relations (edge labels) are:

- on
- inside
- between  _used for door object, if door is between kitchen and livingroom, we have edges `door` between `livingroom` and `door` between `kitchen`_
- close 
- facing
- holds_rh  _edge `character` holds_rh `object` is used to indicate that character holds an object in its right hand_
- holds_lh  _analogue of holds_rh for left hand_

### Properties

- surfaces
- grabbable
- sittable
- lieable
- hangable
- drinkable
- eatable
- recipient
- cuttable
- pourable
- can_open
- has_switch
- readable
- lookable
- containers
- clothes
- person
- body_part
- cover_object
- has_plug
- has_paper
- movable
- cream

### States

- closed
- open
- on
- off
- sitting

## Template

### ExampleExecutor
- script: action object1 object2
- Pre-condition: 
- Post-condition:
    - remove/add undirected edges:
    - remove/add directed edges:
    - state changes:

### FindExecutor
- script: find `object`
- Pre-condition: 
	- exists edge `character` close `object` or for each edge `character` close `object2` exists edge `object2` close `object`  # i.e., either character must be 
		close to `object` or `object` must be close to every object the character is currenty close to
- Post-condition:
    - add undirected edges: `character` close `object`

### WalkExecutor
- script: walk `object`
- Pre-condition: `character` state is not sitting
- Post-condition:
    - remove undirected edges: `character` inside `any_node`, `character` close `any_node`, `character` faces `any_node`
    - add undirected edges: `character` close to object_contain(`object`) [Need to be verified]
    - add directed edges: `character` inside room_of(`object`)
    - add undirected edges: `character` close `object`

### SitExecutor
- script: sit `object`
- Pre-condition: 
	- exists edge `character` close `object`
	- `character` state is not sitting
	- `object` property is sittable
	- `any_node` on `object`
- Post-condition: 
    - add directed edges: `character` on `object`
    - state changes: `character` sitting

### StandUpExecutor
- script: standup
- Pre-condition: `character` state is sitting
- Post-condition: `character` remove state sitting

### GrabExecutor
- script: grab `object`
- Pre-condition: 
	- `object` property is grabbable
	- exists edge `character` close `object`
	- no edge `object` inside `object2` unless `object2` is room or `object2` state is open
	- no edge `character` holds_rh `any_object` or no edge `character` holds_lh `any_object`  // character has at least one free hand 
- Post-condition: 
    - remove directed and undirected edges: `object` any_relation `any_node`
    - add directed edges: `character` holds_rh `object` or `character` holds_lh `object`
    - add undirected edges: `character` close `object2` if there was edge `object` on `object2` (or `object` inside `object2`)  // do not know if this is necessary

### OpenExecutor
- script: open `object`
- Pre-condition: 
	- `object` property is openable and `object` state is closed
	- exists edge `character` close `object`
	- no edge `character` holds_rh `any_object` or no edge `character` holds_lh `any_object`  // character has at least one free hand 
- Post-condition:
    - state changes: `object` state is open

### CloseExecutor (shared with OpenExecutor)
- script: close `object`
- Pre-condition: 
	- `object` property is openable and `object` state is open
	- exists edge `character` close `object`
	- no edge `character` holds_rh `any_object` or no edge `character` holds_lh `any_object`  // character has at least one free hand 
- Post-condition:
    - state changes: `object` state is closed

### PutExecutor
- script: putback `object1` `object2` // means put object on
- Pre-condition: 
	- exists edge `character` holds_lh `object1` or `character` holds_rh `object1`
	- exists edge `character` close `object2`
- Post-condition:
    - remove directed edges: `character` holds_lr `object1` or `character` holds_lr `object2`
    - add undirected edges: `character` close `object2`
    - add directed edges: `object1` on `object2`

### PutInExecutor
- script: putin `object1` `object2`
- Pre-condition:
	- exists edge `character` holds_lh `object1` or `character` holds_rh `object1`
	- exists edge `character` close `object2`
	- `object2` property is not openable or `object2` state is open  // needs adjustment, now one can put something into any object (for openable we check open state); consider possibility of putting sugar in a cup
- Post-condition:
    - remove directed edges: `character` holds_lr `object1` or `character` holds_lr `object2`
    - add undirected edges: `character` close `object2`
    - add directed edges: `object1` inside `object2`

### SwitchOnExecutor
- script: switchon `object`
- Pre-condition: 
	- `object` property is has_switch
	- `object` state is off
	- exists edge `character` close `object`
	- no edge `character` holds_rh `any_object` or no edge `character` holds_lh `any_object`  // character has at least one free hand 
- Post-condition: 
    - state changes: `object` state is on

### SwitchOffExecutor
- script: switchoff `object`
- Pre-condition: 
	- `object` property is has_switch
	- `object` state is on
	- exists edge `character` close `object`
	- no edge `character` holds_rh `any_object` or no edge `character` holds_lh `any_object`  // character has at least one free hand 
- Post-condition: 
    - state changes: `object` state is off

### DrinkExecutor
- script: drink `object`
- Pre-condition:
    - `object` property is drinkable
    - exists edge `character` holds_rh `object` or `character` holds_lh `object`

### TurnToExecutor
- script: TurnTo `object`
- Pre-condition:
	- `character` close `object`
- Post-condition:
	- `character` faces `object`

### LookAtExecutor
- script: LookAt `object`
- Pre-condition:
	- `character` faces `object`

### WipeExecutor
- script: Wipe `object`
- Pre-condition: 
	- `character` close `object`
	- `object` property is `surface`
	- `character` holds_rh or holds_lh `any_node`
- Post-condition:
	- state changes: `object` state is clean