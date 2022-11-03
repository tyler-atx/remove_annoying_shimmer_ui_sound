### Key Findings
_Work journal to help myself or future modders that follow my footsteps with sound editing in this new game_


##### Search Game UI/Ambient Sounds and Toast effects

1. Decompress all sound files
2. Load all sounds in to VLC playlist and manually listen to sounds until the sound is discovered

Bank files are FMOD proprietary format. Here is a [script](https://youtu.be/owvnzePB2iU) to decompress these to waveform.

_ui_global_shimmer_01_ is the offending sound, it is found in the `common/sound/banks/Interface` audio collection.

**Issue**
Recompressing and replacing the whole compressed audio collection is too surgically invasive for such a small mod and will require a much larger mod size, but would be an alternative way to remove this sound from the game entirely by adding a very short, blank sound.  

#####



todo 
- frontend_bookmarks.gui (this has the bookmarks UI element that probably uses the shimmer from animations.gui line 1778). this whole file appears to be commented out
- animations.gui (this file has some global templates like the global 'shimmer' template that may govern all shimmer effects line 534)
- try tech_tree.gui



