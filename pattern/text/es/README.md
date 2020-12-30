https://cl.lingfil.uu.se/~evafo/RuleCompiler/Docs/RuleCompiler.html

How this all works:

[find_tags IN /Users/tallyosborne/personal/pattern/pattern/text/__init__.py]
First it will try to tag sentences using the lexicon.
  This will find an EXACT word and replace its tag.
  Ie:
  llegué VMI

  Will convert llegué to a verb.

Then it runs through the morphology file.
  This will try to update tags based on features of the word.
  Ie:
  NCS ré fhassuf 2 VMI x
  NCS rá fhassuf 2 VMI x
  NCS ría fhassuf 3 VMI x
  NCS rías fhassuf 4 VMI x
  NCS ríamos fhassuf 5 VMI x
  NCS rían fhassuf 4 VMI x

  These rules specify, if a word was tagged NCS (shows as NN in tagged output, it must get converted in the parole conversion), and ends in ré (ré fhassuf 2), then convert it to VMI.

  The order MATTERS.  It will take the FIRST rule that matches.
  ie
  NCS ­as fhassuf 2 NCP x
  NCS rías fhassuf 4 VMI x

  is useless - any word ending in rías will instead be tagged NCP.  The more specific rule needs to come first.


Then it runs through the context tagger.


Then it runs through the context file.
  This will update the tag of the word based on surrounding words.

  Ie:
  NCP VB PREVWD te
  NCP VB PREVWD le
  NCP VB PREVWD me
  NCP VB PREVWD se
  NCS VB PREVWD te
  NCS VB PREVWD le
  NCS VB PREVWD me
  NCS VB PREVWD se

  Convert an NCS to a VB if the previos word was a reflexiv pronoun or direct object pronoun.
  Ie me enctana.

  It seems like in the context file, it will apply the last rule.