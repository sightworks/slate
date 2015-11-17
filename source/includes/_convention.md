# Conventions

## Tokens

Various tokens are used below to describe URLs. All tokens are presented as ``{{token}}`` in the rest of this document.

All of them defined here in the Conventions section are presented as all uppercase: ``{{ROOT}}``.

Name | Description | ``$type`` field contents
---- | ----------- | ------------------------
{{ROOT}} | The top-level URL for the API: ``https://example.digitalxe.com/api/1``. | [``Root``](#root-root)
{{CHANNEL}} | The URL to a channel resource: ``{{ROOT}}/channels/{{channel-id}}`` | [``ChannelResource``](#channel-channelresource)
{{APP}} | The URL to an app resource: ``{{ROOT}}/apps/{{app-id}}`` | [``AppResource``](#app-appresource)
{{GROUP}} | The URL to a group resource: ``{{APP}}/groups/{{group-id}}`` | ``AppGroup``
{{RECORD}} | The URL to a record resource: ``{{APP}}/records/{{record-id}}`` | ``AppRecord``
{{RESOURCE}} | An arbitrary resource URL. | [``Resource``](#resource-resource)
{{KEY}} | A channel key. | N/A

Other tokens, like ``{{channel-id}}`` and ``{{group-id}}`` are described as they appear.

Within a given example code block, each token will carry the same meaning: if multiple objects of the same type are presented in any
code block, like multiple records or groups, each will have it's URL expanded out.
 
## Object Types

For all objects described as being of the [``Resource``](#resource-resource) type, their header includes the token that appears in their ``$type`` property. Others,
which are used primarily for exposition of the rest of the API, do not have this in their header.

