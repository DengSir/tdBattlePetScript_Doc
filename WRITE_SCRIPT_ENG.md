Translated using Google Translate, and tried to make the result more understandable, so in few places the
interpretation might be wrong (e.g., Google Translate gives only half of the sentence or translation doesn't make sense)

Below is an example of how to write a script. Let us suppose that the opponent to defeat is Quintessence of Light,
and the pets in team are Nether Faerie Dragon and Chrominius. The strategy is the following spell
order: Moonfire + Life Exchange + (swap to Chrominius) + Howl + Surge of Power.

First we program a use of Moonfire:

```
ability(Moonfire)
```

> Only one ability cast can be placed on each line.
>
> The command above is interpeted as: Cast Moonfire.

It's also possible to have a conditional casts of abilities, for example, by specifying the weather. The script
below will cast Moonfire only if the weather is not Moonlight:

```
ability(Moonfire) [ !weather(Moonlight) ]
```

> Each command can be conditional, with the condition enclosed in `[]`.
>
> The command above is interpreted as: if the weather is not Moonlight, cast Moonfire.

It's also possible to have several simultaneous conditions for casting of abilities:

```
ability(Moonfire) [ !weather(Moonlight) & self(Nether Faerie Dragon).ability(Moonfire).usable ]
```

> Multiple conditions are separated by `&`, indicating a logical AND. Conditions can't be joined by OR.
>
> The above command is interpreted as: if the weather is not Moonlight and Moonfire is available, cast Moonfire.
>
> Actually, there is no need to determine whether the skills are available. The feasibility of each action is checked automatically, (needs confirmation)
>
> the use of it serves as a simple example.

Now we add the next skill:

```
ability(Moonfire) [ !weather(Moonlight) & self(Nether Faerie Dragon).ability(Moonfire).usable ]
ability(Life Exchange) [ self.ability(Life Exchange).usable ]
```

> The script can be run only step-by-step, each click corresponding to one command to be executed.

Below we will switch pets:

```
ability(Moonfire) [ !weather(Moonlight) & self(Nether Faerie Dragon).ability(Moonfire).usable ]
ability(Life Exchange) [ self.ability(Life Exchange).usable ]
change(Chrominius)
```

The above actions are done when Nether Faerie Dragon is active. It's possible to explicitly state it, for easier understanding of the script:

```
if [ self(Nether Faerie Dragon).active ]
	ability(Moonfire) [ !weather(Moonlight) & self(Nether Faerie Dragon).ability(Moonfire).usable ]
	ability(Life Exchange) [ self.ability(Life Exchange).usable ]
	change(Chrominius)
endif
```

> Use if / endif to include a set of commands.
>
> Indentation is not required, but will improve the readability of the script.

Now let's add Chrominius part:

```
if [ self(Nether Faerie Dragon).active ]
	ability(Moonfire) [ !weather(Moonlight) & self(Nether Faerie Dragon).ability(Moonfire).usable ]
	ability(Life Exchange) [ self.ability(Life Exchange).usable ]
	change(Chrominius)
endif
if [ self(Chrominius).active ]
    ability(Howl) [ !enemy.aura(Howl).exists ]
    ability(Surge of Power)
endif
```

In fact, this battle is very simple, and there is no uncertainty (spells missing, crit leads to death, etc.). The battle sequence is always Moonfire - 
Life Exchange - Swap to Chrominius - Howl - Surge of Power. The fight will end, and the cooldowns on spells will be reset, so we can simplify the script:

```
ability(Moonfire)
ability(Life Exchange)
change(Chrominius)
ability(Howl)
ability(Surge of Power)
```
