# MLDHT - Mainline Distributed Hash Table
[![Build Status](https://travis-ci.org/cit/MLDHT.svg)](https://travis-ci.org/cit/MLDHT)

A Distributed Hash Table (DHT) is a storage and lookup system that is based on a peer-to-peer (P2P) system. The file sharing protocol BitTorrent makes use of a DHT to find new peers without using a central tracker. There are three popular DHT-based protocols: [KAD](https://en.wikipedia.org/wiki/Kad_network), [Vuze DHT](http://wiki.vuze.com/w/Distributed_hash_table) and Mainline DHT. All protocols are based on [Kademlia](https://en.wikipedia.org/wiki/Kademlia) but are not compatible with each other. The mainline DHT is by far the biggest overlay network with around 15-27 million users per day.

MLDHT, in particular, is an [elixir](http://elixir-lang.org/) package that provides a mainline DHT implementation according to [BEP 05](http://www.bittorrent.org/beps/bep_0005.html). It is build on the following modules:

  * `DHTServer` - main interface, receives all incoming messages;
  * `RoutingTable` - maintains contact information of close nodes.

## Getting Started

Learn how to add MLDHT to your Elixir project and start using it.

### Adding MLDHT To Your Project

To use MLDHT with your projects, edit your `mix.exs` file and add it as a dependency:

```elixir
defp application do
  [applications: [:mldht]]
end

defp deps do
  [{:mldht, "~> 0.0.1"}]
end
```

### Basic Usage

If the application is loaded it automatically bootstraps itself into the overlay network. It does this by starting a `find_node` search for a node that belongs to the same bucket as our own node id. In `mix.exs` you will find the boostrapping nodes that will be used for that first search. By doing this, we will quickly collect nodes that are close to us.

If you are curious and would like to see the content of the `RoutingTable` you can use the following command:

```elixir
iex> RoutingTable.Worker.print
```

You can use the following function to find nodes for a specific BitTorrent infohash (e.g. Ubuntu 15.04):

```elixir
iex> "3F19B149F53A50E14FC0B79926A391896EABAB6F"
     |> Base.decode16!
     |> MlDHT.search(fn(node) -> IO.puts "#{inspect node}" end)
```

If you would like to search for nodes and announce yourself to the DHT network use the following function:

```elixir
iex> "3F19B149F53A50E14FC0B79926A391896EABAB6F"
     |> Base.decode16!
     |> MlDHT.search_announce(6881, fn(node) -> IO.puts "#{inspect node}" end)
```

It is also possible search and announce yourself to the DHT network without a TCP port. By doing this, the source port of the UDP packet should be used instead.

```elixir
iex> "3F19B149F53A50E14FC0B79926A391896EABAB6F"
     |> Base.decode16!
     |> MlDHT.search_announce(fn(node) -> IO.puts "#{inspect node}" end)
```

## License

MLDHT source code is released under MIT License.
Check LICENSE file for more information.