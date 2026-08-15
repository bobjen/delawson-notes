# "Erasure Coding"

Lyrics by Bob Jenkins (public), music by Suno. Shared 2026-08-14.

Kept here because it is a compact statement of DeLawson's loss model, and because it
identified three gaps in `DESIGN.md` that the arithmetic had missed: administrative
error, running out of space as a *durability* failure, and the repair-rate stability
condition. See `DESIGN.md` §4.3.4.

> There is no clever coding or cost
> That can guarantee no data loss
> Nothing can protect it from the likes
> Of military action, or asteroid strikes
>
> But there is no need to invoke vicious thugs
> The biggest losses are from software bugs
> And fat-fingered admins who erase
> There is no cure when you run out of space
>
> Single mode of failure's in between
> It's not the best, but not the worst I've seen
> Any 1 of n up does quite well
> Every 1 of n up ... is hell
>
> Triplication's good, on the whole
> When one goes down, replace it, whack-a-mole
> If copies die at p equals 1 in n,
> You lose data at 1 in n cubed in the end.
>
> Triplication loss requires losing three
> Erasure coding is much higher degree
> Triplication loss is p to the third
> Erasure coding's p to the absurd.
>
> Erasure Coding stops random leaks
> It's more like draining a bathtub, so to speak
> If fixes are faster than failures, data's preserved
> Otherwise loss is certain and well deserved
>
> Erasure coding's clever design
> is that two points define a line
> On the line a third point can be placed
> Then any one of three can be erased
>
> A parabola takes not two points, but three,
> Or d points, for curves of higher degree.
> n=k+d, when k more points are placed,
> Then any k of n can be erased.
>
> Erasure Coding stops hardware leaks
> But not admins or management freaks
> So: backup in several unrelated places
> Immortality any 1 of n embraces
>
> Frequently no matter what you do
> You find your worst opponent is you
> The safest place your data could have gotten
> Is the one that you've forgotten
>
> Use Erasure Coding to watch each cloud carefully
> Use several independent clouds for redundancy.
> Immortal data's impossible, tough,
> But perhaps the end of time is good enough

## Note on notation

The verse "n=k+d … then any k of n can be erased" uses *d* for the data count and *k*
for the parity count — the transpose of `DESIGN.md`, which follows the usual
*k*+*m* convention with *k* data and *m* parity. The mathematics is the same.
