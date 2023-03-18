# ByteBrewMAUISDK

## Getting Started

1. Download these packages.
2. In Visual Studio, right click on your project and click "Manage NuGet Packages..."
3. Click the cog wheel / settings button in the top right, next to "Package source:".
4. Add a package source and point it to wherever you have your downloaded packages.
5. Set "Package source" to the one you just made.
6. Go to "Browse" and install the ByteBrewPlugin package. The other two will be included automatically as dependencies, but it's important they're in the folder too.

## Using the SDK

You can add a reference to ByteBrew in your project:
```using ByteBrewPlugin;```

And call functions from the SDK:
```ByteBrew.InitializeByteBrew("key", "secret", "engine");```
