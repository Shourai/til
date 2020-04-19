# Neomutt

## General

These key bindings will work on almost any menu you are in.

    *           Move to last entry
    =           Move to first entry
    :           Enter muttrc command
    >           Scroll down one line
    <           Scroll up one line
    [           Scroll up half a page
    ]           Scroll down half a page
    ?           Help
    ;           Apply next function to tagged messages only
    !           Invoke command in subshell
    return      Select the current entry
    ESC + /     Search up
    /           Search down
    H           Move to top of page
    j           Move to next entry
    k           Move to previous entry
    CTRL + l    redraw screen
    L           Move to bottom of page
    M           Move to middle of page
    n           Move to next match of search
    q           Exit menu
    t           Tag current entry
    z           Move to next page
    Z           Move to previous page

## Index Menu

When you first open mutt you are in the index menu.

    &                Link tagged message to current one
    #                Break the thread in two
    %                Toggle whether mailbox will be rewritten
    .                List mailboxes with new mail
    $                Save changes to mailbox
    @                Display full address of sender
    |                Pipe message to a shell command
    ESC + tab        Jump to previous new or unread message
    return           Display message
    tab              Jump to next new or unread message
    a                Create alias from message sender
    b                Remail message to another user
    ESC + c          Open a different folder (Read Only)
    c                Open a different folder
    ESC + C          Make text/plain copy
    C                Copy message to another file/mailbox
    ESC + d          Delete all messages in subthread
    d                Delete current message
    CTRL + D         Delete all messages in thread
    D                Delete messages matching a pattern
    ESC + e          Use the current message as a template for a new one
    e                Edit the raw message
    CTRL + E         Edit attachment content type
    f                Forward message with comments
    CTRL + F         Wipe passphrase from memory
    F                Toggle the important flag for message
    g                Reply to all
    G                Retrive mail from POP server
    h                Display message and toggle header weeding
    j                Move to next undeleted message
    ESC + k          Mail a PGP key
    k                Move to previous undeleted message
    CTRL + K         Extract supported public keys
    ESC + l          Show current limit pattern
    l                Only show messages matching a pattern
    L                Reply to specific mailing list
    m                Compose new message
    ESC + n          Jump to next subthread
    CTRL + N         Jump to next thread
    N                Toggle new flag
    o                Sort messages
    O                Sort messages in reverse order
    Q                Query external program for addresses
    q                Save changes to mailbox and quit
    r                Reply to message
    CTRL + P         Jump to previous thread
    ESC + p          Jump to previous subthread
    p                Print current message
    ESC + P          Check for classic PGP
    P                Jump to parent message in thread
    CTRL + R         Mark current thread as read
    R                Recall a postponed message
    ESC + r          Mark current subthread as read
    ESC + s          Save text/plain copy and delete
    s                Save message/attachment to mailbox/file
    ESC + t          Tag current thread
    CTRL + T         Untag messages matching a pattern
    T                Tag messages matching pattern
    ESC + u          Undelete all messages in subthread
    u                Undelete current entry
    CTRL + U         Undelete all messages in thread
    U                Undelete messages matching pattern
    ESC + v          Collapse/uncollapse current thread
    v                Show mime attachments
    ESC + V          Collapse/uncollapse all threads
    V                Show mutt version number and date
    w                Set a status flag
    W                Clear status flags from message

## Flags

When viewing messages in the index menu, you will see various flags such as `N` which mean
the messages is new and `D` which means that the message is to be deleted. This is a short
list of those flags.

    !   Message is flagged
    *   Message is tagged
    +   Message is To: you and only you
    C   Message is Cc: to you
    d   Message has attachments marked for deletion
    D   Marked for deletion
    F   Message is From: you
    K   Contains PGP key
    L   Message is sent to a subscribed mailing list
    n   Thread contains new messages (Only when thread is collapsed)
    N   Message is new
    o   Thread contains old messages (Only when thread is collapsed)
    O   Message is old
    P   Message is PGP encrypted
    r   Message has been replied to
    s   Message is signed
    S   Message is signed and verified
    T   Message is to you and has others in To: or Cc:

