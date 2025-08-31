### **Check if a List is a Set**

```isabelle
fun isSet:: "'a list ⇒ bool"
where
"isSet []=True" |
"isSet (x#xs)=(if (List.member xs x) then False else (isSet xs))"
```

---

### **Remove Duplicates from a List**

```isabelle
fun clean:: "'a list ⇒ 'a list"
  where
"clean [] = []"|
"clean (x#xs) =(if (List.member xs x) then (clean xs) else (x # clean xs))"
```

---

### **Delete an Element from a List**

```isabelle
fun delete:: "'a ⇒'a list ⇒ 'a list"
  where
"delete a [] = []"|
"delete a (x#xs) = (if (a = x) then xs else (x # delete a xs)) "
```

---

### **Intersection of Two Lists**

```isabelle
fun intersect:: "'a list ⇒ 'a list ⇒ 'a list"
  where
"intersect [] l2 =  []"|
"intersect (x#xs) ys = (if (List.member ys x ∧ ¬List.member xs x) then (x # (intersect xs ys)) else (intersect xs ys))"
```

---

### **Union of Two Lists**

```isabelle
fun union :: "'a list ⇒ 'a list ⇒ 'a list"
  where
"union [] l2 = l2" |
"union (x#xs) ys = (if  (¬List.member xs x ∧ ¬List.member ys x)  then (x # (union xs ys))  else (union xs ys))"
```

---

### **Check if a List is a Subset of Another**

```isabelle
fun inclusion :: "'a list ⇒ 'a list ⇒ bool"
  where 
"inclusion [] _ = True" |
"inclusion (x#xs) ys = (List.member ys x ∧ inclusion xs ys)"
```

---

### **Check Equality of Two Lists**

```isabelle
fun equal :: "'a list ⇒ 'a list ⇒ bool" 
  where
"equal [] [] = True" |
"equal [] _ = False" |
"equal _ [] = False" |
"equal xs  ys = (inclusion xs ys ∧ inclusion ys xs)"
```

---

### **Check if an Element Exists in a List**

```isabelle
fun contains::"'a => 'a list => bool"
where 
"contains _ [] = False" |
"contains e (x # xs) = (if (x=e) then True else (contains e xs))"
```

---

### **Remove First Occurrence of an Element from a List**

```isabelle
fun oneDelete:: "'a => 'a list => 'a list"
where 
"oneDelete e [] = []" |
"oneDelete e (a#l) = (if e=a then l else a#(oneDelete e l))"
```

---

### **Check if Two Lists are Permutations**

```isabelle
fun isPermut:: "'a list ⇒ 'a list ⇒ bool"
where
"isPermut [] [] = True" |
"isPermut (_#_) [] = False" |
"isPermut [] (_#_) = False" |
"isPermut (x#xs) l = (if (contains x l) then (isPermut xs (oneDelete x l)) else False)"
```

---

### **Add Two Tables**

```isabelle
fun fold_table_add :: "varNbOccurrence ⇒ varNbOccurrence ⇒ varNbOccurrence" 
  where
 "fold_table_add t1 [] = t1"|
 "fold_table_add t1 ((s2,i2)#t2s) =  (case (assoc s2 t1) of Some(y) ⇒ (fold_table_add (modify s2 (y+i2) t1) (t2s) ) | None ⇒ (fold_table_add (modify s2 (i2) t1)(t2s))) "
```

---

### **Add Two Compiled Expressions**

```isabelle
fun add_tuple :: "compiledExp ⇒ compiledExp ⇒ compiledExp"
  where
" add_tuple (i1,t1) (i2,t2) = (i1+i2,fold_table_add t1 t2)"
```

---

### **Subtract Two Tables**

```isabelle
fun fold_table_sub :: "varNbOccurrence ⇒ varNbOccurrence ⇒ varNbOccurrence" 
  where
 "fold_table_sub t1 [] = t1"|
 "fold_table_sub t1 ((s2,i2)#t2s) =  (case (assoc s2 t1) of Some(y) ⇒ (fold_table_sub (modify s2 (y-i2) t1) (t2s) ) | None ⇒ (fold_table_sub (modify s2 (-i2) t1)(t2s))) "
```

---

### **Subtract Two Compiled Expressions**

```isabelle
fun sub_tuple :: "compiledExp ⇒ compiledExp ⇒ compiledExp"
  where
" sub_tuple (i1,t1) (i2,t2) = (i1-i2,fold_table_sub t1 t2)"
```

---

### **Compile an Expression**

```isabelle
fun compile :: "expression ⇒ compiledExp"
  where
  "compile (Constant c) = (c, [])" |
  "compile (Variable x) = (0, [(x, 1)])" |
  "compile (Sum e1 e2) = add_tuple (compile e1) (compile e2)" |
  "compile (Sub e1 e2) = sub_tuple (compile e1) (compile e2) "
```

---

### **Check Equivalence of Two Expressions**

```isabelle
fun equiv_expr :: "expression ⇒ expression ⇒ bool"
  where
    "equiv_expr ex1 ex2 =( compile ex1 = compile ex2)"
```

---

### **Evaluate a Compiled Expression**

```isabelle
fun evalCompiled :: "compiledExp ⇒ symTable ⇒ int"
  where
  "evalCompiled (c, t) st = c + sum_list (map (λ(x, n). n * (case assoc x st of None ⇒ 0 | Some v ⇒ v)) t)"
```

---

### **Filter an Address Based on a Chain**

```isabelle
fun filter:: "address ⇒ chain ⇒ bool"
  where
"filter a [] = False" |
"filter a ((Accept al)#r) = (if (List.member al a) then True else (filter a r))"|
"filter a ((Drop al)#r) = (if (List.member al a) then False else (filter a r))" 
```

---

### **Get All Addresses in a Chain**

```isabelle
fun addresses_in_chain :: "chain ⇒ address list"
  where
"addresses_in_chain [] = []" |
"addresses_in_chain ((Drop al)#r) = al @ addresses_in_chain r" |
"addresses_in_chain ((Accept al)#r) = al @ addresses_in_chain r"
```

---

### **Check if an Address is in a Chain**

```isabelle
fun member :: "chain ⇒ address ⇒ bool"
  where
"member c a = (List.member (addresses_in_chain c) a)"
```

---

### **Check Equality of Two Chains**

```isabelle
fun equal :: "chain ⇒ chain ⇒ bool"
  where
"equal c1 c2 = List.list_all (λa. filter a c1 = filter a c2) (addresses_in_chain c1 @ addresses_in_chain c2)"
```

---
### **Check for BAD Condition**

```isabelle
fun BAD::"(symTable * inchan * outchan) ⇒ bool "
  where
 "BAD (_, _, []) = False" |
 "BAD (t , i , (X a)#r ) =( if (a = 0) then True else (BAD(t,i,r)))"|
 "BAD (t , i, _#r) = BAD(t,i,r)"
```

---

### **First Version of `san0`: Check Statement Safety**

```isabelle
fun san0 :: "statement ⇒ bool" where
  "san0 (Exec _) = False" |
  "san0 (Seq s1 s2) = (san0 s1 ∧ san0 s2)" |
  "san0 (If _ s1 s2) = (san0 s1 ∧ san0 s2)" |
  "san0 _ = True"
```

---

### **Second Version of `san1`: Improved Statement Safety**

```isabelle
fun san1 :: "statement ⇒ bool" where
  "san1 (Exec (Constant n)) = (n ≠ 0)" |
  "san1 (Exec _) = False" |
  "san1 (Seq s1 s2) = (san1 s1 ∧ san1 s2)" |
  "san1 (If _ s1 s2) = (san1 s1 ∧ san1 s2)" |
  "san1 _ = True"
```

---

### **Datatype for Sign Information**

```isabelle
datatype sign_info = Consts "int list" | NotZero | Unknown
type_synonym sign_symTable = "(string * sign_info) list"
```

---

### **Retrieve Sign Information**

```isabelle
fun get_sign_info :: "string ⇒ sign_symTable ⇒ sign_info" where
  "get_sign_info x [] = Consts [(-1)]" |
  "get_sign_info x ((y, v) # t) = (if x = y then v else get_sign_info x t)"
```

---

### **Update Sign Symbol Table**

```isabelle
fun set_sign_symTable :: "string ⇒ sign_info ⇒ sign_symTable ⇒ sign_symTable" where
  "set_sign_symTable x v t = (x, v) # t"
```

---

### **Merge Sign Information**

```isabelle
fun merge_sign_info :: "sign_info ⇒ sign_info ⇒ sign_info" where
  "merge_sign_info (Consts l1) (Consts l2) = Consts (remdups (l1 @ l2))" |
  "merge_sign_info (Consts l) NotZero =
     (if List.member l 0 then Unknown else NotZero)" |
  "merge_sign_info NotZero (Consts l) =
     (if List.member l 0 then Unknown else NotZero)" |
  "merge_sign_info NotZero NotZero = NotZero" |
  "merge_sign_info Unknown _ = Unknown" |
  "merge_sign_info _ Unknown = Unknown"
```

---

### **Merge Symbol Tables**

```isabelle
fun merge_symTables :: "sign_symTable ⇒ sign_symTable ⇒ sign_symTable" where
  "merge_symTables symt1 symt2 =
     (let vars = remdups (map fst symt1 @ map fst symt2) in
      map (λx.
            (let v1 = get_sign_info x symt1;
                 v2 = get_sign_info x symt2;
                 v = merge_sign_info v1 v2
             in (x, v))
          ) vars)"
```

---

### **Add Signs**

```isabelle
fun add_signs :: "sign_info ⇒ sign_info ⇒ sign_info" where
  "add_signs (Consts l1) (Consts l2) =
     Consts (remdups (concat (map (λn1. map (λn2. n1 + n2) l2) l1)))" |
  "add_signs NotZero NotZero = Unknown" |
  "add_signs NotZero (Consts l) = Unknown" |
  "add_signs (Consts l) NotZero = Unknown" |
  "add_signs _ _ = Unknown"
```

---

### **Subtract Signs**

```isabelle
fun sub_signs :: "sign_info ⇒ sign_info ⇒ sign_info" where
  "sub_signs (Consts l1) (Consts l2) =
     Consts (remdups (concat (map (λn1. map (λn2. n1 - n2) l2) l1)))" |
  "sub_signs NotZero NotZero = Unknown" |
  "sub_signs NotZero (Consts l) = Unknown" |
  "sub_signs (Consts l) NotZero = Unknown" |
  "sub_signs _ _ = Unknown"
```

---

### **Evaluate Sign Information**

```isabelle
fun eval_sign_info :: "expression ⇒ sign_symTable ⇒ sign_info" where
  "eval_sign_info (Constant n) _ = Consts [n]" |
  "eval_sign_info (Variable x) symt = get_sign_info x symt" |
  "eval_sign_info (Sum e1 e2) symt =
     (let v1 = eval_sign_info e1 symt;
          v2 = eval_sign_info e2 symt
      in add_signs v1 v2)" |
  "eval_sign_info (Sub e1 e2) symt =
     (if e1 = e2 then Consts [0]
      else
        (let v1 = eval_sign_info e1 symt;
             v2 = eval_sign_info e2 symt
         in sub_signs v1 v2))"
```

---

### **Evaluate Condition with Sign Information**

```isabelle
fun evalC_sign_info :: "condition ⇒ sign_symTable ⇒ bool option" where
  "evalC_sign_info (Eq e1 e2) symt =
     (let v1 = eval_sign_info e1 symt;
          v2 = eval_sign_info e2 symt
      in (case (v1, v2) of
            (Consts l1, Consts l2) ⇒
              (if l1 = l2 then Some True
               else if List.null (List.filter (λx. List.member l2 x) l1) then Some False
               else None) |
            _ ⇒ None))"
```

---

### **Assume Condition is True**

```isabelle
fun assume_true :: "condition ⇒ sign_symTable ⇒ sign_symTable" where
  "assume_true (Eq (Variable x) (Constant n)) symt =
     set_sign_symTable x (Consts [n]) symt" |
   "assume_true (Eq (Constant n) (Variable x)) symt =
     set_sign_symTable x (Consts [n]) symt" |
  "assume_true _ symt = symt"
```

---

### **Assume Condition is False**

```isabelle
fun assume_false :: "condition ⇒ sign_symTable ⇒ sign_symTable" where
  "assume_false (Eq (Variable x) (Constant n)) symt =
     (if n = 0 then set_sign_symTable x NotZero symt else symt)" |
   "assume_false (Eq (Constant n) (Variable x)) symt =
     (if n = 0 then set_sign_symTable x NotZero symt else symt)" |
   "assume_false _ symt = symt"
```

---

### **Safety Analysis Helper Function**

```isabelle
fun san2aux :: "statement ⇒ sign_symTable ⇒ (bool * sign_symTable)" where
  "san2aux Skip symt = (True, symt)" |
  "san2aux (Read x) symt = (True, set_sign_symTable x Unknown symt)" |
  "san2aux (Print e) symt = (True, symt)" |
  "san2aux (Aff x e) symt =
     (let v = eval_sign_info e symt;
          symt2 = set_sign_symTable x v symt
      in (True, symt2))" |
  "san2aux (Seq s1 s2) symt =
     (let (b1, symt1) = san2aux s1 symt;
          (b2, symt2) = san2aux s2 symt1
      in (b1 ∧ b2, symt2))" |
"san2aux (If c s1 s2) symt =
     (case evalC_sign_info c symt of
        Some True ⇒ san2aux s1 symt |
        Some False ⇒ san2aux s2 symt |
        None ⇒
          (let symt1 = assume_true c symt;
               symt2 = assume_false c symt;
               (b1, symt1') = san2aux s1 symt1;
               (b2, symt2') = san2aux s2 symt2;
               symt' = merge_symTables symt1' symt2'
           in (b1 ∧ b2, symt')))" |
"san2aux (Exec e) symt =
     (let v = eval_sign_info e symt in
      (case v of
         Consts l ⇒ (if List.member l 0 then (False, symt) else (True, symt)) |
         NotZero ⇒ (True, symt) |
         Unknown ⇒ (False, symt)))"
```

---

### **Final Safety Analysis**

```isabelle
fun san2 :: "statement ⇒ bool" where
  "san2 s = (let (r, _) = san2aux s [] in r)"
```

---

### **Lemmas for `san2`**

```isabelle
lemma correctionSan3: "san2 s = True ⟹ ∀ inch . ¬ BAD (evalS s ([], inch, []))"
lemma completudeSan2: " (∀ inch . ¬ BAD (evalS s ([], inch, []))) ⟹ san2 s = True"
```

---
### **Define Transaction Identifier and Message Types**

```isabelle
type_synonym transid= "nat*nat*nat"

datatype message= 
  Pay transid nat  
| Ack transid nat
| Cancel transid
```

---

### **Define Transaction and Transaction Database**

```isabelle
type_synonym transaction= "transid * nat"
type_synonym transBdd = "(transid, (nat option * nat option * bool)) table"
```

---

### **Process a Single Message**

```isabelle
fun traiterMessage :: "message ⇒ transBdd ⇒ transBdd"
  where
  "traiterMessage (Pay tid amc) tbd =  (case assoc tid tbd of
                                        None ⇒ modify tid (Some amc, None, False) tbd | 
                                        Some (Some prev_amc, amm, canceled) ⇒
                                          if canceled ∨ (prev_amc ≥ amc) then tbd
                                          else modify tid (Some amc, amm, canceled) tbd |
                                        Some (None, amm, canceled) ⇒
                                          if canceled then tbd
                                          else modify tid (Some amc, amm, False) tbd) "|
  "traiterMessage (Ack tid amm) tbd =  (case assoc tid tbd of
                                        None ⇒ modify tid (None, Some amm, False) tbd |
                                        Some (amc, Some prev_amm, canceled) ⇒
                                          if canceled ∨ (prev_amm ≤ amm) then tbd
                                          else modify tid (amc, Some amm, canceled) tbd |
                                        Some (amc, None, canceled) ⇒
                                          if canceled then tbd
                                          else modify tid (amc, Some amm, False) tbd)"|
  "traiterMessage (Cancel tid) tbd =   (case assoc tid tbd of
                                        None ⇒ modify tid (None, None, True) tbd |
                                        Some (amc, amm, _) ⇒ modify tid (amc, amm, True) tbd)"
```

---

### **Export Valid Transactions**

```isabelle
fun export :: "transBdd ⇒ transaction list" 
  where
  "export [] = []" |
  "export ((tid, (Some amc, Some amm, False))#tbd) =
          (if amc > 0 ∧ amc ≥ amm then (tid, amc) # export tbd else export tbd)" |
  "export (_#tbd) = export tbd"
```

---

### **Process a List of Messages**

```isabelle
fun traiterMessageListaux :: "message list ⇒ transBdd ⇒ transBdd"
  where
  "traiterMessageListaux [] tbd = tbd" |
  "traiterMessageListaux (m#ms) tbd = traiterMessageListaux ms (traiterMessage m tbd)"

fun traiterMessageList :: "message list ⇒ transBdd" 
  where
"traiterMessageList m = traiterMessageListaux m []"
```

---

### **Lemma 1: Validated Transactions are Positive**

```isabelle
lemma all_validated_transactions_positive:
  "List.member (export tbd) (tid, amc) ⟶ amc > 0"
  quickcheck [size=6, tester=narrowing, timeout=120]
  nitpick [timeout=1] (* no counterexample *)
  apply (induct tbd)
   apply auto
  apply (simp add: member_rec(2))
  by (smt (verit) export.elims list.distinct(1) list.inject member_rec(1) old.prod.inject)
```

---

### **Lemma 2: Distinctness of Transactions**

```isabelle
lemma distinct_inter:
  "distinct (export (traiterMessageList ml)) ⟹ distinct (export (traiterMessageList (a # ml)))"
  apply (induct ml)
  prefer 2
  apply (rename_tac mess ml)
  sorry

lemma distinct_validated:
  "distinct (export (traiterMessageList ml))"
  quickcheck [size=6, tester=narrowing, timeout=120]
  nitpick [timeout=1] (* no counterexample *)
  apply (induct ml)
   apply (case_tac "a # ml")
    apply fastforce
   apply fastforce
  using distinct_inter by blast
```

---

### **Lemma 3: Canceling Transactions Removes Them**

```isabelle
lemma transaction_can_be_canceled:
  "List.member (export tbd) (tid, amc) ⟶ ¬ List.member (export (traiterMessage (Cancel tid) tbd)) (tid, amc)"
  quickcheck [size=6, tester=narrowing, timeout=120]
  nitpick [timeout=1] (* no counterexample *)

  oops
```

---

### **Lemma 4: Canceling is Permanent**

```isabelle
lemma cancel_is_permanent:
 "tbd' = traiterMessageList (l1@[Cancel tid]@l2) ⟶ ¬ List.member (export tbd') (tid, amc)"
  quickcheck [size=6, tester=narrowing, timeout=120]
  nitpick [timeout=1] (* no counterexample *)

  oops
```

---

### **Lemma 5: Valid Transactions Have Pay and Ack**

```isabelle
lemma valid_transaction_if_pay_and_ack:
  "amc > 0 ∧ amc ≥ amm ∧ List.member ms (Pay tid amc) ∧ List.member ms (Ack tid amm) ∧ 
                                                        ¬ List.member ms (Cancel tid) ∧
   (∀ amc'. List.member ms (Pay tid amc') ⟶ amc' ≤ amc) ⟶ List.member (export (traiterMessageList ms)) (tid, amc)"
  quickcheck [size=6, tester=narrowing, timeout=120]
  nitpick [timeout=1] (* no counterexample *)

  oops
```

---

### **Lemma 6: Valid Transactions Have Pay and Ack in Database**

```isabelle
lemma validated_transactions_have_pay_and_ack:
  "List.member (export tbd) (tid, amc) ⟶
   (∃ amm. List.member tbd (tid, Some amc, Some amm, False) ∧ amc ≥ amm)"
  quickcheck [size=6, tester=narrowing, timeout=120]
  nitpick [timeout=1] (* no counterexample *)

  oops
```

---

### **Lemma 7: Client Gave Less**

```isabelle
lemma client_gave_less:
  "m2 < m1 ⟹ 
   (export (traiterMessageList (mlist1 @ [Pay tid m1] @ mlist2 @ [Pay tid m2] @ mlist3)) =
      export (traiterMessageList (mlist1 @ [Pay tid m1] @ mlist2 @ mlist3)))"
  quickcheck [size=6, tester=narrowing, timeout=120]
  nitpick [timeout=1] (* no counterexample *)

  oops
```

---

### **Lemma 8: Merchant Gave More**

```isabelle
lemma merchant_gave_more:
  "m2 > m1 ⟹ 
   ( export (traiterMessageList (mlist1 @ [Ack tid m1] @ mlist2 @ [Ack tid m2] @ mlist3)) =
       export (traiterMessageList (mlist1 @ [Ack tid m1] @ mlist2 @ mlist3)))"
  quickcheck [size=6, tester=narrowing, timeout=120]
  nitpick [timeout=1] (* no counterexample *)

  oops
```

---

### **Lemma 9: Validated Transaction Amounts Remain Constant**

```isabelle
lemma validated_transaction_amount_constant:
  "List.member (export tbd) (tid, amc) ⟶
   tbd = traiterMessageList ms ⟶
   (∃ amc'. List.member (export tbd) (tid, amc') ∧ amc' = amc)"
  quickcheck [size=6, tester=narrowing, timeout=120]
  nitpick [timeout=1] (* no counterexample *)

  oops
```

---

### **Lemma 10: Transaction Amount Proposed by Client**

```isabelle
lemma amount_in_validated_transaction_proposed_by_client:
  " List.member (export (traiterMessageList ml)) (tid, amc) ⟶
     (∃ ms. tbd = traiterMessageList ms ∧ List.member ms (Pay tid amc))"
  quickcheck [size=6, tester=narrowing, timeout=120]
  nitpick [timeout=1] (* no counterexample *)

  oops
```

---
### **Basic Definitions**

#### 1. **Binary Trees**

```isabelle
datatype tree = Empty | Node int tree tree
```

#### 2. **Options and Lists**

```isabelle
datatype 'a option = None | Some 'a
fun nth :: "nat ⇒ 'a list ⇒ 'a option" where
  "nth _ [] = None" |
  "nth 0 (x#xs) = Some x" |
  "nth n (x#xs) = nth (n - 1) xs"

fun member :: "'a ⇒ 'a list ⇒ bool" where
  "member _ [] = False" |
  "member e (x#xs) = (if e = x then True else member e xs)"
```
#### 3. **Suppress an Element from a List**

```isabelle
fun suppress :: "'a ⇒ 'a list ⇒ 'a list" where
  "suppress _ [] = []" |
  "suppress e (x#xs) = (if e = x then suppress e xs else x # suppress e xs)"
```

#### 4. **Check if a List is Sorted**

```isabelle
fun sorted :: "nat list ⇒ bool" where
  "sorted [] = True" |
  "sorted [x] = True" |
  "sorted (x#y#xs) = (x ≤ y ∧ sorted (y#xs))"
```

#### 5. **Concatenate Lists**

```isabelle
fun concat :: "'a list ⇒ 'a list ⇒ 'a list" where
  "concat [] ys = ys" |
  "concat (x#xs) ys = x # concat xs ys"
```

#### 6. **FIFO Queue**

```isabelle
type_synonym 'a queue = "'a list × 'a list"

fun add :: "'a ⇒ 'a queue ⇒ 'a queue" where
  "add e (l1, l2) = (e#l1, l2)"

fun head :: "'a queue ⇒ 'a option" where
  "head (l1, x#l2) = Some x" |
  "head ([], []) = None" |
  "head (l1, []) = head ([], rev l1)"

fun pop :: "'a queue ⇒ 'a queue" where
  "pop (l1, x#l2) = (l1, l2)" |
  "pop ([], []) = ([], [])" |
  "pop (l1, []) = pop ([], rev l1)"
```

---

#### 7. **Properties of `nth` and `member`**

```isabelle
lemma nth_member: "nth n xs = Some e ⟹ member e xs"
  by (induction xs arbitrary: n) auto
```

#### 8. **Concatenation Properties**

```isabelle
lemma concat_assoc: "concat (concat xs ys) zs = concat xs (concat ys zs)"
  by (induction xs) auto

lemma concat_length: "length (concat xs ys) = length xs + length ys"
  by (induction xs) auto
```

#### 9. **Suppress and Sorted**

```isabelle
lemma suppress_sorted: "sorted xs ⟹ sorted (suppress e xs)"
  by (induction xs) auto
```

#### 10. **FIFO Queue Properties**

```isabelle
lemma add_head: "head (add e q) = (if q = ([], []) then Some e else head q)"
  by (cases q) auto

lemma pop_add: "pop (add e q) ≠ q"
  by (cases q) auto
```

---

### **Additional Structures**

#### 11. **Security Policies**

Define a simple security structure and access validation:

```isabelle
datatype role = Visiteur | Employe | ResponsableSecurite
type_synonym policy = "(role × role) list"

fun infEgal :: "role ⇒ role ⇒ bool" where
  "infEgal Visiteur _ = True" |
  "infEgal Employe Visiteur = False" |
  "infEgal Employe _ = True" |
  "infEgal ResponsableSecurite r = (r = ResponsableSecurite)"
```

---
### **Basic Functions on Binary Trees**

#### 1. **Tree Size**: Calculate the number of nodes in a tree

```isabelle
fun size :: "'a tree ⇒ nat" where
  "size Empty = 0" |
  "size (Node _ l r) = 1 + size l + size r"
```

#### 2. **Tree Height**: Calculate the maximum depth of a tree

```isabelle
fun height :: "'a tree ⇒ nat" where
  "height Empty = 0" |
  "height (Node _ l r) = 1 + max (height l) (height r)"
```

#### 3. **Tree Member**: Check if an element exists in the tree

```isabelle
fun member :: "'a ⇒ 'a tree ⇒ bool" where
  "member _ Empty = False" |
  "member x (Node y l r) = (x = y ∨ member x l ∨ member x r)"
```

#### 4. **Tree Insert**: Insert an element into a binary search tree (BST)

```isabelle
fun insert :: "int ⇒ int tree ⇒ int tree" where
  "insert x Empty = Node x Empty Empty" |
  "insert x (Node y l r) =
     (if x < y then Node y (insert x l) r
      else if x > y then Node y l (insert x r)
      else Node y l r)"
```

#### 5. **In-Order Traversal**: Return elements in in-order

```isabelle
fun inorder :: "'a tree ⇒ 'a list" where
  "inorder Empty = []" |
  "inorder (Node x l r) = inorder l @ [x] @ inorder r"
```

#### 6. **Pre-Order Traversal**: Return elements in pre-order

```isabelle
fun preorder :: "'a tree ⇒ 'a list" where
  "preorder Empty = []" |
  "preorder (Node x l r) = x # preorder l @ preorder r"
```

#### 7. **Post-Order Traversal**: Return elements in post-order

```isabelle
fun postorder :: "'a tree ⇒ 'a list" where
  "postorder Empty = []" |
  "postorder (Node x l r) = postorder l @ postorder r @ [x]"
```

#### 8. **Mirror a Tree**: Swap left and right subtrees

```isabelle
fun mirror :: "'a tree ⇒ 'a tree" where
  "mirror Empty = Empty" |
  "mirror (Node x l r) = Node x (mirror r) (mirror l)"
```

---

### **Useful Lemmas**

#### 1. **Mirror Lemma**: Double mirroring returns the original tree

```isabelle
lemma mirror_mirror: "mirror (mirror t) = t"
  by (induction t) auto
```

#### 2. **Size Lemma**: Insertion increases size

```isabelle
lemma size_insert: "size (insert x t) = (if member x t then size t else size t + 1)"
  by (induction t) auto
```

#### 3. **In-Order Traversal of Insert**: Preservation of sorted property

```isabelle
lemma inorder_insert: "sorted (inorder t) ⟹ sorted (inorder (insert x t))"
  by (induction t) auto
```

#### 4. **Height Lemma**: Mirroring preserves height

```isabelle
lemma height_mirror: "height (mirror t) = height t"
  by (induction t) auto
```

---

### **Binary Search Tree (BST) Specific Functions**

#### 1. **Find Minimum Element**

```isabelle
fun find_min :: "int tree ⇒ int option" where
  "find_min Empty = None" |
  "find_min (Node x Empty _) = Some x" |
  "find_min (Node _ l _) = find_min l"
```

#### 2. **Find Maximum Element**

```isabelle
fun find_max :: "int tree ⇒ int option" where
  "find_max Empty = None" |
  "find_max (Node x _ Empty) = Some x" |
  "find_max (Node _ _ r) = find_max r"
```

#### 3. **Tree Deletion**: Remove an element from a BST

```isabelle
fun delete :: "int ⇒ int tree ⇒ int tree" where
  "delete _ Empty = Empty" |
  "delete x (Node y l r) =
     (if x < y then Node y (delete x l) r
      else if x > y then Node y l (delete x r)
      else case (find_min r) of
              None ⇒ l |
              Some m ⇒ Node m l (delete m r))"
```

---

### **Important Tree Properties**

1. **Height and Size Relation**:
    
    ```isabelle
    lemma height_le_size: "height t ≤ size t"
      by (induction t) auto
    ```
    
2. **In-Order Traversal for BST is Sorted**:
    
    ```isabelle
    lemma inorder_sorted: "sorted (inorder t)"
      (* Assumes that the tree is a valid BST. *)
    ```
    
3. **Member After Insert**:
    
    ```isabelle
    lemma member_insert: "member x (insert y t) ⟷ (x = y ∨ member x t)"
      by (induction t) auto
    ```
    

---
