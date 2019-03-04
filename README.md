# VirtualHome
VirtualHome is a platform to simulate complex household activities via Programs. Given an initial environment describing an apartment and a program depicting a sequence of actions, VirtualHome executes the program generating a video of the activity together with useful data for activity understanding or planning.

Check out more details of the environmnent and platform in [virtualhome.org]. VirtualHome has been used in:

- VirtualHome: Simulating HouseHold Activities via Programs
- Synthesizing Environment-Aware Activities via Activity Sketches

# QuickStart
Clone the repository and download the simulator
```
git clone https://xavierpuigf@bitbucket.org/mboben/virtualhome.git
instructions to copy the executable
```
Run `sh run_example.sh` to generate an example video. You can check more examples in.



## Contents


## Dependencies

## Dataset generation

### dataset strucutre

programs_processed_precond_nograb_morepreconds/
├── initstate
├── withconds
└── withoutconds

- go to dataset_augmentation/ and run `python augment_dataset_affordances.py`
- run `python augment_dataset_locations.py`
- go to root dir, and run `python check_programs.py` with check_2('dataset_augmentation/augmented_location_augmented_affordance_programs_processed_precond_nograb_morepreconds', graph_path=translated_path)
- got o dataset_augmentation/ and run `python perturbate_dataset.py`
- go to root dir, and run `python check_programs.py` with check_2('dataset_augmentation/perturb_augmented_location_augmented_affordance_programs_processed_precond_nograb_morepreconds', graph_path=translated_path)

## Examples

For how to use the code, see `example.py` file

Example scripts are located in `example_scripts` folder 

## Files

Folder `resources` contains json files with information about:

### Class name equivalence (`class_name_equivalence.json`)

This file contains a dictionary mapping from names occurring in scripts to "equivalent" names (class_name) found in Unity scenes. For example,

`"fruit": ["watermelon", "apple", "banana"]`

means that for

`[Find] <fruit> (1)`

executor will choose between objects named fruit, watermelon, apple, or banana.

Note that adding keys to value list is not necessary (i.e., `"fridge": ["fridge"]` or `"fruit": ["fruit", "watermelon", "apple", "banana"]`)
since this is done automatically.

### Object properties (`object_properties.json`)

This file contains a dictionary mapping from object names to their properties, see `environment.Property` enum class for a list of all
supported properties. Example:

`"oven": ["CAN_OPEN", HAS_SWITCH", CONTAINERS", HAS_PLUG"]`

### Object placing (`object_placing.json`)

This file contains a dictionary from object names to a list of their possible placings. A placing is defined as a dictionary with keys 

* `destination`: specifies a destination object
* `relation`: either "ON" or "INSIDE" (will be supporded soon)
* `room`: room name where the desination object must occur (or null for object in any room) (will be supporded soon)

Example

```json
{
	"juice": [{
		"destination": "kitchen_counter",
		"relation": "ON",
		"room": null
	},
	{
		"destination": "table",
		"relation": "ON",
		"room": "kitchen"
	}]
}
```

