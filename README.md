# Tetris Bot Protocol (TBP)

Tetris-playing computer programs have recently become more popular with the
development of *Zetris* (mat1jaczyyy), a port of *MisaMino* (misakamm) to the
official Tetris game *Puyo Puyo Tetris*. Since then, new programs have been
developed such as *Cold Clear* (MinusKelvin), *Tetras* (traias), *Wataame*
(ameyasu), and *Hikari* (SoRA_X7). Currently, all of these independently solve
the problem of interfacing with the host game. This duplicates a lot of work
between these projects, especially since this interfacing code is generally not
made public due to concerns about such programs being used to cheat in online
Tetris games. This repository aims to specify a common interface for
communication between a Tetris-playing program and a Tetris frontend, similar to
how the Universal Chess Interface solves a similar problem for Chess.
