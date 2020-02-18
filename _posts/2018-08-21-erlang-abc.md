---
layout: post
title: Erlang ABC
---
The following is a checklist for Erlang beginer.

Terms
-----

### Numbers: integers and floats

```erlang
12. % integer()
$A.
16#ffff % hexadecimal integer
3.14159 % float number
12 + 4
1234567890 * 99342424
4 / 2 % 2.0
7 div 2 % 3
15 rem 4 % 3
N bsl K
N bsr K
X band Y
X bor Y
X bxor Y
bnot Y
```

### Binaries: a low level sequence of bits or bytes

```erlang
<<Seqment1, ..., SegmanetN>>
%% Seqment syntax:
%%   Data
%%   Data:Size
%%   Data/TypeSpecifiers
%%   Data:Size/TypeSpecifiers
<<1, 2, 3, 4>>
<<"hello">>
```

### Atoms: begin with a lower-case letter, like lisp\'s symbol

```erlang
abc
xyz@H123
'hello'
nonode@nohost
```

### Variables: begin with upper-case letter

```erlang
Var = 90.
_Ignored
_
```

### Pids: process identifier

```erlang
self().
spawn/1, 2, 3, 4
spawn_link/1, 2, 3, 4
spawn_opt/4
```

### References: unique in an Erlang runtime system

```erlang
make_ref().
```

### Tuples: for storing a fixed number of terms, separated by commas and enclosed in curly brackets

```erlang
{1, 'hello', {9, 5}}.
{size, 42} % tagged tuple
```

### Lists: for storing a variable number of terms, separated by commas and enclosed in square brackets

```erlang
[1, 3, abc, "hello", {5, 9}].
"abcd". % strings are lists
```

### PropLists: property list

```erlang
P = [{a, 7}, {b, 50}, {def, 60}].
Num = proplists:get_value(a, P).
```

### Maps: a variable number of key-value associations

```erlang
M1 = #{name=>adam, age=>24, date=>{july,29}}.
maps:get(name, M1).
#{name := Name} = M1. % use pattern matching

M2 = maps:update(age, 25, M1).
M3 = M1#{age=>26}.
M4 = M1#{age := 27}. % only updating existing value
map_size(M1).
```

### Term Comparisons

number \< atom \< reference \< fun \< port \< pid \< tuple \< map \< nil \< list \< bit string

### Fun functions

```erlang
Fun1 = fun(X) -> X+1 end. % lambda syntax
```

### Records

```erlang
-record(customer, {name="<anonymous>", address, phone}). % record declaration, rd in erl
R0 = #customer{} % create record
R1 = #customer{phone="55512345"}
R2 = #customer{name="Sandy", address="Chris Town", phone="555123"}

% access record fields
R1#customer.name
R1#customer.address
R1#customer.phone

% update field
R3 = R1#customer(phone="143")

print_customer(#customer(name=Name, address=Addr, phone=Phone)) when Phone =/= undefined ->
    io:format("Contact: ~s at ~s.~n", [Name, Phone]).

% record info
io:format("~p~n", [record_info(fields, customer)]).
```

Modules
-------

```erlang
-module(Module).
-export(Functions).
-import(Module,Functions).
-compile(Options).
-vsn(Vsn).
-on_load(Function).
-behaviour(Behaviour).

-type my_type() :: atom() | integer().
-spec my_function(integer()) -> integer().
```

Preprocess: Macros
----------

```erlang
-define(PI, 3.14).
-define(pair(X, Y), {X, Y}).

-include("filename.hrl")
-include_lib("kernel/include/file.hrl")

-ifdef(MacroName).
-ifndef(Macroname).
-else.
-endif.
```

Pattern Matching: more powerful assignment
------------------------------------------

Pattern matching occurs: evaluating an expression of the form Lhs = Rhs
calling a function matching a pattern in a case or receive primitive

Pattern expressions:

```erlang
{A, B} = {12, apple}. % single assignment
```

Primitives
----------

### Case
```erlang
case Expression of
    Pattern1 [when Guard1] -> Expr_seq1;
    Pattern2 [when Guard2] -> Expr_seq2;
    ...
end
```

### If

```erlang
if
    Condition1 ->
        Action1;
    Condition2 ->
        Action2;
end
```

BIFs: Built-In Functions
------------------------

List BIFs

```erlang
atom_to_list(A).
float_to_list(F).
integer_to_list(I).
list_to_atom(L).
list_to_float(L).
list_to_integer(L).
hd(L).
tl(L).
length(L).
tuple_to_list(T).
list_to_tuple(L).
```

Tuple BIFs

```erlang
tuple_to_list(T).
list_to_tuple(L).
element(N, T).
setelement(N, T, Val).
size(T).
tuple_size(T).
```

Cocurrent Programming
---------------------

```erlang
Pid = spawn(Module, FunctionName, ArgumentList).

Pid ! Message. % send Message to process

receive
    Message1 [when Guard1] ->
        Actions1;
    Message2 [when Guard2] ->
        Actions2;
after Time ->
        TimeoutBody
end.
```

Distributed Programming
-----------------------

```erlang
spawn(Node, Mod, Func, Args).
spawn_link(Node, Mod, Func, Args).
monitor_node(Node, Flag).
node().
nodes().
node(Item).
disconnect_node(Nodename).
```

BIFs
----

```erlang
abs(Number).
alive(Name, Port).
apply(Module, Function, ArgumentList).
atom_to_list(Atom).
binary_to_list(Binary).
binary_to_list(Binary, Start, Stop).
binary_to_term(Binary).
erlang:check_process_code(Pid, Module).
concat_binary(ListOfBinaries).
date().
erlang:delete_module(Module).
disconnect_node(Node).
element(N, Tuple).
erase().
erase(Key).
exit(Reason).
exit(Pid, Reason).
float(Number).
float_to_list(Float).
get().
get(Key).
erlang:get_cookie().
get_keys().
group_leader().
group_leader(Leader, Pid).
halt().
erlang:hash(Term, Range).
hd(List).
integer_to_list(Integer).
is_alive().
length(List).
link(Pid).
list_to_atom(AsciiIntegerList).
list_to_binary(Asciiintegerlist).
list_to_float(Asciiintegerlist).
list_to_integer(Asciiintegerlist).
list_to_pid(Asciiintegerlist).
list_to_tuple(List).
erlang:load_module(Module, Binary).
make_ref().
erlang:math(Function, Number, [,Number]).
erlang:module_loaded(Module).
monitor_node(Node, Flag).
node().
node(Arg).
nodes().
now().
open_port(PortName, PortSettings).
pid_to_list(Pid).
erlang:pre_loaded().
process_flag(Flag, Option).
process_info(Pid).
process_info(Pid, Key).
processes().
erlang:purge_module(Module).
put(Key, Value).
register(Name, Pid).
registered().
round(Number).
erlang:set_cookie(Node, Cookie).
self().
setelement(Index, Tuple, Value).
size(Object).
spawn(Module, Function, Argumentlist).
spawn(Node, Module, Function, Argumentlist).
spawn_link(Module, Function, Argumentlist).
spawn_link(Node, Module, Function, Argumentlist).
split_binary(ListOfbinaries, Pos).
statistics(Type).
term_to_binary(Term).
throw(Any).
time().
tl(List).
trunc(Number).
tuple_to_list(Tuple).
unlink(Pid).
unregister(Name).
whereis(Name).
```

Standard Libraries
------------------

use \`erl -man io\` to read the manual

### io module

```erlang
format([Dev], F, Args).
get_chars([Dev], P, N).
get_line([Dev], P).
nl([Dev]).
parse_exprs([Dev], P).
parse_form([Dev], P).
put_chars([Dev], L).
read([Dev], P).
write([Dev], Term).
```

### file module

```erlang
read_file(File).
write_file(File, Binary).
get_cwd().
set_cwd(Dir).
rename(From, To).
make_dir(Dir).
del_dir(Dir).
list_dir(Dir).
file_info(File).
consult(File).
open(File, Mode).
close(Desc).
position(Desc, N).
truncate(Desc).
```

### lists module

```erlang
append(L1, L2).
append(L).
concat(L).
delete(X, L).
flat_length(L).
flatten(L).
keydelete(Key, N, LTup).
keysearch(Key, N, LTup).
keysort(N, LTup).
member(X, L).
last(L).
nth(N, L).
reverse(L).
reverse(L1, L2).
sort(L).
```

### code module

```erlang
set_path(File).
load_file(File).
is_loaded(Module).
ensure_loaded(Module).
purge(Module).
soft_purge(Module).
all_loaded().
priv_dir(Module).
```

Erlang Shell
------------

``` {.shell}
erl # start the erlang shell

## erlang shell functions
help().
h().
v(N).
cd(Dir).
ls().
ls(Dir).
pwd().
q(). # init:stop().
i().
rp(v(-1)).
memory().
```

Ctrl-C: BREAK menu Ctrl-G: user switch command menu, command: h or ?

Testing
-------

```erlang
-include_lib("eunit/include/eunit.hrl")

% test function must end with "_test"

% eunit:test(ModuleName) or ModuleName:test() run test
```

OTP
---

Directory structure: doc, ebin, include, priv and src. (ebin is
essential)

setup OTP application:

1.  mkdir -p appname-1.0/{doc,ebin,include,priv,src}
2.  add metadata file: appname.app to ebin
3.  write module to implement application behaviour

``` {#ebin/appname.app .erlang}
{application, appname,
 [{description, "application example"},
  {vsn: "1.0"},
  {modules, [appname]},
  {registered, [appname]},
  {applications, [kernel, stdlib]},
  {mod, {app, []}}
 ]}.
```

```erlang {#src/appname.erl .erlang}
-module(appname).
-behaviour(application). % application:behaviour_info(callbacks)

-export([
    start/2,
    stop/1
]).

start(_Type, _StartArgs) ->
    ok.

stop(_State) ->
    ok.
```

```shell
erlc -o ebin src/*.erl
erl -pa ebin
% application:start(appname). % startup application
```

```erlang
observer:start(). % launch the monit tool
debugger:start(). % launch the debugger, erlc +debug_info -o ebin src/*.erl
```

Distribution
------------

```shell
erl -name simple
erl -sname simple
```

```erlang
net_adm:ping('b@mybox.home.net').
```

Mnesia
------

``` {.shell}
erl -mnesia dir '"/tmp/mnesia_store"' -name mynode
```

```erlang
mnesia:create_schema([node()]).
mnesia:start().
mnesia:info().
mnesia:create_table(user, [{attributes, record_info(fields, user)}])
```

Perf
----

cprof

```erlang
cprof:start().
% 
cprof:pause().
cprof:analyse(ModuleName).
cprof:stop().
```

fprof

List Comprehensions
-------------------

```erlang
[X || X <- [1, 2, a, 3, 4, b, 5, 6], X > 3].
```
