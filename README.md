# Solidity (Cheat Sheet)

> [!NOTE]  
> This is a cheat sheet on Solidity (mostly notes-to-self). They are incomplete by default.

## Mappings

Source: ChatGPT

Mappings in Solidity are a type of data structure that allows you to store and retrieve values
associated with specific keys. They are often used for associative arrays or dictionaries.

1. **Storage and Access**: Mappings are used to store key-value pairs. You can access the value
   associated with a key using the syntax `mappingName[key]`.

1. **No Length or Iteration**: Mappings in Solidity do not have a length property or a way to
   iterate over all key-value pairs. Unlike arrays, you cannot directly determine the number of
   entries in a mapping or iterate through them without additional data structures or helper
   functions.

1. **Custom Getters and Setters**: While Solidity provides automatic getters for public variables,
   including mappings, these getters only allow you to retrieve values by their key. To work with
   mappings more extensively (e.g., getting all keys or values), you need to implement custom
   functions.
