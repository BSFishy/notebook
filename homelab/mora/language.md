# mora language

so mora gets its own language. its made up of blocks and statements a block is
either named or not and contains more blocks and statements. at the syntax
level, everything is quite loosy goosy and then ill go back in sema and clean
everything up. its easier that way, trust me. actually i dont actually know, but
we'll figure it out with this project.

so a statement is just an identifier equal to an expression. expressions are
lisp-style expressions because they're trivial to parse and i dont want to kill
myself building expression driven parsers and shit like that. so statements
might look something like this:

```txt
my_property = (secret (config "my_secret_name") "my default value");
```

something like that. that should make it trivial to parse everything so we can
move on to bigger and better things.

blocks are some identifiers and expressions or whatever then curly braces then
more of the same. so maybe something like this

```txt
service (config "my_service_name") {
  name = (config "my_service_name");
  image = "my_image_url@sha256:xxxxxx";

  volumes {
    "/data" {
      name = "media-server";
    }
  }
}
```

or something along those lines. that should give me a lot of flexibility and
ability to make it very adaptable to whatever service i want to be running.

one thing i need to keep in mind is how to handle ad-hoc setup, that is setup as
part of the wizard process with the manager. it would be easy to force the
configuration to need to be statically evaluable, but that would mean that i
outright couldn't have configuration as part of the wizard process. so i need to
be able to mark an expression as something that is dynamic.

so the most likely thing is that i'll convert the configuration into some sort
of json structure then send it to the mora manager and then the manager will do
what it needs to do. the question is how much processing should i be doing
before sending it. the orchestrator (which at this point is probably a bad name
for it) should be able to validate that the configuration is valid before even
making a connection to the manager.

so im thinking that it honestly doesn't evaluate any of the expressions. i guess
that's something that the manager should outright take care of. but again, the
orchestrator should statically validate that the config is actually valid. that
should allow the manager to handle persistence of all manual configuration then
only prompt for the incremental changes. and it absolutely needs to evaluate
ad-hoc, as that is required for configuration through the wizard.

for dynamic templating in the manager, it seems i can use go's `text/template`
and possibly sprig for additional functions. that should give me everything i
need for templating files dynamically for deployment. good to go :)

## configuration variables

so in the examples above, i mention `(config "variable_name")`. i think this
will need to have its own special top-level block. something like

```txt
config "variable_name" {
  name = "My human-readable variable";
  help = "A longer piece of text that describes what this is and what it needs
to do. Tokens can span multiple lines but idk if that will actually be used like
it is here. URLs should also show up as links in the UI.";

  secret = true;
}
```

i think this is pretty important so that i can specify config variable info in a
single centralized place. it should also give me the opportunity to like group
together variables. for example, i would want to group together cloudflare api
key and email in a single page to get them at the same time.
