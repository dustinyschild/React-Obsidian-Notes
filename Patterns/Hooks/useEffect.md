useEffect should be used SPARINGLY

- only use useEffect for state **synchronization**. Furthermore, think about whether state synchronization is needed, can you derive from existing state instead of setting a new state?

- anything that can be driven by events should be driven by events triggered by the user