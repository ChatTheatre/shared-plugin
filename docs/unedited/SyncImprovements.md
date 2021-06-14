%META:TOPICINFO{author=\"kallea\" date=\"1162240948\" format=\"1.0\"
version=\"1.2\"}% %META:TOPICPARENT{name=\"SyncSystem\"}%
[NIPStyle]{.twiki-macro .INCLUDE}

# Sync Improvements

Also known as \"Ye Mighty Todo.\"

For Sync 2.1 \* A merry:spawn:create callback that\'s called before
spawning a new object that didn\'t exist on a slave. If you return 1,
it\'ll be done. If you return 0, the object won\'t be created and, as a
side-effect, no configuration will occur (nor will the two relevant
merry callbacks be done). It would be (a) for completeness and (b)
potentially useful to be able to mark objects as \"never (again)
recreate this object, even though it exists on the master (HQ)\" \* An
addition to the PENDING:SyncState called \"external-callbacks\" which is
a map of objects-\>({ function object, function name\[, \...\] }). When
an object has been configured, if the map contains entries for that
object, those function calls are made on-the-fly by the system. \* More
informative spawn:initialize info (source).

For Sync ?.? \* Let the slaves acquire the master object, or a generated
diff (as a string) between the XML copies of the master and slave, and
present that in the +synctool popup for better judgement calls.

\-- Main.KalleAlm - 06 Oct 2006
