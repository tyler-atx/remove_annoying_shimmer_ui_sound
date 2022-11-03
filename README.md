# Remove Annoying Shimmer UI Sound

![thumbnail](https://github.com/tyler-atx/remove_annoying_shimmer_ui_sound/blob/main/thumbnail.png)

Victoria 3 Mod that removes an annoying UI sound that loops. This will help players who do not use the in-game music and hear all the sounds.  

The shimmer sound can appear from multiple UI elements. The bookmarks, the tech tree, and others. Removing one source of the shimmer sound can temporarily relieve the player, but it will often reappear shortly after. This mod will remove the shimmer sound effect from the parent template for many UI elements.



---

# Work Journal    

_Work journal to help myself or future modders that follow my footsteps with sound editing in this new game_

---

### Search Game UI/Ambient Sounds and Toast effects

1. Decompress all sound files
2. Load all sounds in to VLC playlist and manually listen to sounds until the sound is discovered

The compressed audio collection bank files are FMOD proprietary format. Here is a [script](https://youtu.be/owvnzePB2iU) to decompress these to waveform.

_ui\_global\_shimmer\_01_ is the offending sound, it is found in the `common/sound/banks/Interface` audio collection.

> **Project-level Issue**   
> The interface bank collection is 14,096kB and the entire collection needs to be replaced to override a small 3-second sound with a mute sound effect.         
> Recompressing and replacing the whole compressed audio collection is too surgically invasive for such a small mod and will require a much larger mod size.    

The next step is to search for a way to disable the sound without removing it from the compressed audio bank.

---

### Search for frontend calls to the shimmer effect   

`gui/shared/animations.gui` contains templates for the `shimmer` style which is used on multiple UI elements including the tech tree buttons.

Below is the code that governs the shimmer template.

```
File: game\gui\shared\animations.gui
508: template shimmer {
509: 	modify_texture = {
510: 		name = "glow"
511: 		texture = "gfx/interface/animation/shimmer.dds"
512: 		blend_mode = colordodge
513: 		translate_uv = { 0 0 }
514: 		block "shimmer_visibility" {}
515: 	}
516: 
517: 	state = {
518: 		trigger_on_create = yes
519: 		name = show
520: 		next = shimmer
521: 		duration = 1
522: 
523: 		modify_texture = {
524: 			name = "glow"
525: 			translate_uv = { -1 -1 }
526: 		}
527: 	}
528: 
529: 	state = {
530: 		name = shimmer
531: 		next = pause
532: 		duration = 2
533: 		start_sound = {
534: 			soundeffect = "event:/SFX/UI/Global/shimmer"
535: 		}
536: 		
537: 		bezier = { 0 0.9 1 0.4 }
538: 
539: 		modify_texture = {
540: 			name = "glow"
541: 			translate_uv = { 1 1 }
542: 		}
543: 
544: 		block "shimmer_animation_properties" {}
545: 	}
546: 
547: 	state = {
548: 		name = pause
549: 		next = shimmer
550: 		duration = 0
551: 		delay = 6
552: 
553: 		modify_texture = {
554: 			name = "glow"
555: 			translate_uv = { -1 -1 }
556: 		}
557: 	}	
558: }

```

Above, you can see that the button cycles through 3 modes: 
1. `show` for 1 second only on creation (non-repeating)
2. `shimmer` for 2 seconds
3. `pause` for 6 seconds

It loops between 2 and 3 _forever_, driving players crazy.

We will [remove](https://github.com/tyler-atx/remove_annoying_shimmer_ui_sound/commit/bbc2a53cb75593ebb79ab6a2907a1d96d6a59ed6#diff-24dc24c4046921d7570d91aeb322733a055faa6224874aae1d53d99b6388665bL533) the `start_sound` block on `lines 533, 534, 535` but allow the visual shimmer to continue. 
