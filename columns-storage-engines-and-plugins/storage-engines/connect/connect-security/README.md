# CONNECT - Security

The use of the CONNECT engine requires the <a undefined>FILE</a> privilege for
["outward"](/kb/en/inward-and-outward-tables/#outward-tables) tables. This should not be an important restriction. The use of
CONNECT "outward" tables on a remote server seems of limited interest without
knowing the files existing on it and must be protected anyway. On the other
hand, using it on the local client machine is not an issue because it is always
possible to create locally a user with the `FILE` privilege.