prompt for qwen2.5 14b:
```
Does the recording description '{}' probably describe a non-instrumental, abstract sound effect? Exclude descriptions of loops or raw recordings, I'm interested only in synthetic SFX-style sounds. Answer only with a literal 'yes' or 'no', no other symbols.
```

57288 samples filtered from `freesound_meta.csv`
51591/5697 split
47G in total

following samples from the csv are missing:
```
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/469787.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/486541.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/497639.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/509068.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/504186.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/512355.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/468193.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/489183.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/509212.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/497221.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/505240.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/473175.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/503282.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/473369.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/493047.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/510163.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/470204.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/485390.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/486746.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/487040.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/468277.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/467917.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/467153.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/504123.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/493724.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/489344.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/471983.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/469996.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/515057.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/492109.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/481052.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/489844.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/475584.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/472757.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/474874.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/469198.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/491092.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/466971.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/508096.flac": No such file or directory
scp: stat local "/mnt/uesb/mgr/freesound/audio_files/test/474610.flac": No such file or directory
```
### sfx short
17761/1935 train/val samples