bcast package for Go
====================

Broadcasting on a set of channels in Go. Go channels offer different usage patterns but not ready to use broadcast pattern.
This library solves the problem in direct way. Each routine keeps member structure with own input channel and single for all
members output channel. Central dispatcher accepts broadcasts and resend them to all members.

Usage example below.

Firstly import package and create broadcast group. You may create any number of groups for different broadcasts:

			import (
						 "github.com/grafov/bcast"
			)

			group := bcast.NewGroup() // create broadcast group

			go bcast.Broadcasting(0) // accepts messages and broadcast it to all members

Now join to the group from different goroutines:

			member1 := group.Join() // joined member1 from one routine
			member1.Send("test message") // broadcast message

Method `bcast.Send` accepts `interface{}` type so any values may be broadcasted.

			member2 := group.Join() // joined member2 form another routine
			val := member1.Recv() // broadcasted value received

Another way to receive broadcasted messages is listen input channel of the member.

			val := <-*member1.In // each member keeps pointer to its own input channel

It may be convenient for example when `select` used.

See more examples in a test suit `bcast_test.go`.

License
-------

Library licensed under GPLv3. See LICENSE.

Project status
--------------

[![Is maintained?](http://stillmaintained.com/grafov/bcast.png)](http://stillmaintained.com/grafov/bcast)
