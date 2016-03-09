# my_elixir_notes
Here I just write down notes, Ideas and solution I come over in learning Elixir especially based on nerves/bakeware

## How I use IEX --remsh in development
In order to check the setup in a nerves box I put following into the vm.args file
```elixir
# rel/vm.args
## Name of the node
-name <node-name>@<ip-address>

## Cookie for distributed erlang
-setcookie secret
```
In the application I use Nerves.Io.Ethernet to setup eth0
```elixir
      case Ethernet.setup(:eth0) do
        {:ok, _pid} -> Logger.info "dynamic IP #{Nerves.IO.Ethernet.settings(:eth0)[:ip]}"
        _           -> {:ok, _pid} = Ethernet.setup( :eth0, mode: "static", ip: "10.0.0.5", router:
                                     "10.0.0.1", mask: "16", subnet: "255.255.0.0", mode: "static",
                                     dns: "8.8.8.8 8.8.4.4")
                                     Logger.info "static IP #{Nerves.IO.Ethernet.settings(:eth0)[:ip]}"
      end
```
Now I can use on my development machine
```elixir
iex --name console@<local-ip> --remsh <node-name>@<ip-address> --cookie secret 
```

## What is my development setup
Well I am using vim on my mac. Since I am trying a lot of things depending on the hardware of the nerves box I did setup a raspberry pi with raspian an a full blown elixir stack. I connect to this box with ssh from my mac. Since I feel it more pleasent to use vim on the mac I just put an alias in my zsh.
```sh
alias push_rpi='rsync --progress -arv --exclude=".git" --exclude="_images" --exclude="_build" --exclude="deps" ~/nerves/project1/ root@raspi:~/project1'
```
So I can edit everything on the mac and just sent a :!push_rpi from vim an have it deployed to the development box.
And when everything works I deploy it using bake to the nerves box 

... To be continued
