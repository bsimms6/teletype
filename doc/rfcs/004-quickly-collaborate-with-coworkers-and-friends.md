# Quickly collaborate with coworkers and friends

## Status

Proposed

## Summary

A user can add any of their recent collaborators to a list of "trusted collaborators." Once two people have added each other to their list of trusted collaborators, they can establish future collaboration sessions with each other from directly within Atom.

## Motivation

We hope to encourage more collaboration by reducing the barriers to entry. Specifically, we want to reduce the number of steps that exist between A) deciding that you *want* to collaborate and B) *actually* collaborating.

Today, the transition from A to B involves the following steps:

1. Host shares a portal
2. Host copies portal URL to their clipboard
3. Host switches from Atom to third-party communication tool (e.g., Slack, IRC)
4. Host pastes portal URL into third-party communication tool
5. Guest clicks portal URL, and the operating system hands control to Atom, and Atom joins the portal

With the ability to invite trusted collaborators to your portal from within Atom, we reduce the host's set-up process from 4 steps to 3 steps:

1. Host shares a portal
2. Host selects person in their list of trusted collaborators
3. Host invites person to join the portal
4. Guest accepts invitation to join portal, and Atom joins the portal

And perhaps we can reduce the host's set-up process even further:

1. Host selects a person in their list of trusted collaborators
2. Host invites the selected person to a portal, and Atom automatically shares a portal for the host and invites the selected person
3. Guest accepts invitation to join portal, and Atom joins the portal

## Explanation

### Accept portal invitations from a trusted collaborator

Teletype provides a list of your recent collaborators. Each time a guest joins your portal, Teletype adds them to your list of recent collaborators. Each time you join a portal, Teletype adds the host to your list of recent collaborators.

You can select a past collaborator and inform Teletype that you're willing to receive portal invitations from them directly within Atom, thus identifying them as a "trusted collaborator."

*TODO* - Add mockup

## Invite a trusted collaborator to your portal

Teletype presents your list of trusted collaborators sorted alphabetically by username.

You can select a trusted collaborator (an "invitee" in this context) and invite them to join your portal. If you're not already hosting a portal, Teletype automatically creates a portal for your workspace.

If the invitee has added you as a trusted collaborator, they see an invitation inside Atom informing them that you have invited them to join your portal, and Teletype asks them if they want to join it. If the invitee is offline when you invite them to join your portal, they'll see your invitation the next time they're online. [A]

If the invitee chooses to join the portal, the portal opens in the invitee's workspace (just as it does when joining a portal via a URL).

*TODO* - Add mockup

## Privacy and safety considerations

1. **Safely decline invitations** - When a host invites you to join their portal, the host sees the invitation in a "pending" status. If you decline to join the portal, no change of status is communicated to the host; the host continues to see the invitation in a "pending" status. [[motivation](https://github.com/atom/teletype/pull/344#discussion_r175911322)]
2. **Opt-in to receiving invitations (or don't)** - Choosing to accept in-Atom portal invitations is entirely opt-in.
    1. Adding a person to your trusted collaborator list is a private action. Another user has no way of knowing whether or not they are one of your trusted collaborators. [[motivation](https://github.com/atom/teletype/pull/344#discussion_r175911322)]
    2. You'll only receive portal invitations from trusted collaborators. You control this "whitelist" and can change it at any time. Teletype won't show you any kind of invitation or request from people outside your list of trusted collaborators. [[motivation](https://github.com/atom/teletype/pull/344#pullrequestreview-105405268)]
3. **Opt-out safely at any time** - At any time, you can remove a person from your list of trusted collaborators.   
    1. You'll no longer receive invitations from them within Atom.
    2. They observe no indication that you have removed them. [[motivation](https://github.com/atom/teletype/pull/344#discussion_r175911322)]
    3. Because they observe no indication that you have removed them, they may still have you in their list of trusted collaborators (i.e., their list of people that _they_ are willing to accept in-Atom portal invitations from). If they invite you to a portal using their trusted collaborators list, they see the invitation in a "pending" status (i.e., the same status seen prior to you removing them from your trusted collaborators list).
4. **Easily prevent future invitations** - When you decline an invitation from a trusted collaborator, Teletype presents the option to block future invitations from that person (i.e., to remove them from your trusted collaborators).

### Mockups

#### Simultaneously participating in multiple portals

While probably not very common, it's possible to host a portal while also participating as a guest in other portals.

*TODO* - Add mockup

#### Other mockups?

*TODO*

## Out of scope

In the interest of getting the highest impact functionality in users' hands as quickly as possible and then iterating based on real-world feedback, the following functionality is out of scope for this RFC, but may be addressed in follow-up releases.

1. Showing online/offline status (i.e., presence) for recent collaborators and trusted collaborators [[discussion](https://github.com/atom/teletype/pull/344#pullrequestreview-105405268)]
2. UX enhancements to the list of recent collaborators and trusted collaborators
    - Changing the sort order for your list of collaborators (e.g., sort by how recently you've collaborated)
    - Limiting the size of your list of trusted collaborators. (In the meantime, you can remove trusted collaborators to reduce the size of the list.)
    - Filtering your list of trusted collaborators
    - Marking certain trusted collaborators as "favorites"
3. Selecting a trusted collaborator and asking to join their portal
4. Selecting multiple trusted collaborators and inviting them to your portal simultaneously. (In the meantime, you can select a collaborator and invite them, then select another collaborator and invite them, and so on.)
5. Providing option to join portal in a new window [[discussion](https://github.com/atom/teletype/pull/344#discussion_r175569812)]
6. Automatically preventing portal invitations from trusted collaborators that have been flagged as suspended/spammy on github.com

## Drawbacks

1. Imagine you code together every day with a particular coworker. When you add them as a trusted collaborator, and you invite them to join your portal, and they don't join right away, you can't tell if it's because they're not online, or if they declined your invitation, or if they didn't see your invitation, or if they don't have you listed as a trusted collaborator. If you were expecting them to join your portal, you have to reach out to them via some other communication medium to find out why they haven't joined. (Unfortunately, we're unaware of an [alternative approach](#rationale-and-alternatives) that would allow us to improve this experience for you and your coworker without _also_ increasing the ability for bad actors to use Teletype as an avenue for harassment.)
2. Once Teletype provides a list of past collaborators, people may want to be able to chat with those collaborators from within Teletype. While we may eventually want to support chat, any chat-related functionality must exist in service of Teletype's primary vision of "making it as easy to code together as it is to code alone." We'll need to be diligent to avoid scope creep.
3. Today, when a host shares a portal, the host sends their peer information to Teletype so that guests can query Teletype to determine how to connect to the host. Teletype has no peering information for other users (e.g., users that a host might want to invite to their portal). In order for Teletype to communicate a portal invitation to a user, Teletype will need some way of sending a message to the user. Depending on the technical solution we choose, we may incur increased server-side resource consumption and/or we may take on additional operational complexity.

## Rationale and alternatives

##### Why is this approach the best in the space of possible approaches?

With the approach described above, we believe Teletype can provide a more streamlined set-up process while also preventing harassment. To establish a collaboration session with a coworker or friend directly within Atom, we each add each other as trusted collaborators, and then we can start collaborating at any time with just a couple clicks. With this mutual consent model, you're in complete control of which users can communicate with you.

##### What other approaches have been considered and what is the rationale for not choosing them?

1. **Invite anyone to collaborate by username**: Teletype could allow you to enter a person's GitHub username (or their email address) to invite them to your portal. This would remove the need for sharing a URL via a third-party service in order to collaborate, and it would reduce the need to maintain a list of trusted collaborators (i.e., you could just enter a username each time you want to collaborate). However, it would make it possible for any GitHub user to cause invitations to appear inside your Atom instance. This introduces a vector for harassment, so we're avoiding this approach.
2. **Invite anyone to collaborate by username, and ignore invitations from blocked users**: To reduce the harassment concerns described in the previous bullet, Teletype could require that you grant it additional permissions so that it can read your [blocked user settings](https://developer.github.com/v3/users/blocking/) to determine whether to show you an invitation from a particular user. With this approach, if a user blocks a person on github.com, Teletype could automatically prevent invitations from the blocked person. However, this approach introduces tradeoffs that we'd prefer to avoid:
    - Users would need to grant Teletype substantially greater permissions in order for Teletype to check users' block lists. Due to coarse-grained OAuth scopes, users would need to [grant Teletype permission to read/write their private email addresses, update their github.com profile, etc.][user-scope-request] Teletype would no longer be able to simply require [read-only permission to each user's public info][scopeless-request].
    - Some users would still likely receive unwanted portal invitations from users they haven't formally blocked. A popular developer might receive a ton of unwanted invitations, and/or a harasser might send invitations to people that haven't yet blocked them. Because of this, Teletype would likely still need to provide some additional safety/spam prevention on top of integrating with the user's github.com block list.
3. **Ask for permission to send in-Atom portal invitations to a user**: To avoid seeing in-Atom portal invitations from the general public, Teletype could require that you first request a person's permission to send them portal invitations [[discussion](https://github.com/atom/teletype/pull/344#issuecomment-375663822)]. (This approach is similar to a "friend request" on Facebook.) To avoid receiving friend requests from users that you've blocked on github.com, Teletype would need permission to read your block list. This approach therefore suffers from the same issues described in the previous bullet.
4. **Invite any fellow org member to collaborate**: Teletype could allow you to invite other users that you're already associated with in some way (e.g., fellow collaborators on a GitHub repository, fellow members of a GitHub organization or team). This would remove the need for sharing a URL via a third-party service in order to collaborate, and it would reduce the need to maintain a list of trusted collaborators, and it would reduce the harassment vector described in the previous bullets. However, it would introduce tradeoffs that we'd prefer to avoid:
    - Teletype would need additional permissions (i.e., [OAuth scopes](https://developer.github.com/apps/building-oauth-apps/scopes-for-oauth-apps/#available-scopes)) to fetch the list of users you're associated with. (Today, Teletype  uses a scopeless token and therefore has no access to your private information.)
    - Some users belong to organizations with thousands of members. Just because you're a member of the same organization as someone else doesn't mean that you're comfortable seeing portal invitations from them.
5. **Add support for audio before adding in-Atom portal invitations**: Teletype could add support for audio before adding support for establishing collaboration sessions directly within Atom. When collaborating with people in different physical locations, you'll likely need audio support in order to collaborate. Since Teletype doesn't currently provide audio support, these collaborators will still need a third-party solution for audio. If you're already relying on a third-party solution for audio, you can often use that same solution to send your collaborators your portal URL, which reduces the need for Teletype to maintain a list of trusted collaborators. However, there are still many [scenarios where it's helpful to quickly invite a trusted collaborator even if Teletype doesn't yet support audio](https://github.com/atom/teletype/pull/344#discussion_r175319913).

##### What is the impact of not doing this?

People will collaborate less often. Given the additional steps needed to start collaborating, there will be more instances where people decide that it's not worth the effort (i.e., people will enjoy rich collaboration less often).

## Unresolved questions

##### What unresolved questions do you expect to resolve through the RFC process before this gets merged?

- If I invite a past collaborator to join my portal, and that person has multiple Atom windows open, can we show the invitation only in the frontmost window?
- If I invite a past collaborator to join my portal, and that person has Atom windows open on multiple computers, are we OK with showing the invitation on each computer?
- Where will the lists of recent and trusted collaborators appear in the UI? How do I show/hide these lists?

##### What unresolved questions do you expect to resolve through the implementation of this feature before it is released in a new version of the package?

- What architecture/services/libraries will we use to communicate portal invitations?
- In order to provide the functionality described above, does teletype-server need to persist your list of trusted collaborators, or can we meet these needs while only storing this data locally?
- If the host invites a trusted collaborator to their portal and then closes the portal before the invitee joins the portal, what should an invitee see/experience to inform them that the portal no longer exists?

##### What related issues do you consider out of scope for this RFC that could be addressed in the future independently of the solution that comes out of this RFC?

See [out of scope](#out-of-scope) section above.

---

[A] A user is **online** if they have Atom open, and Teletype is installed, and they are signed into Teletype, and their network allows them to successfully communicate with `api.teletype.atom.io`. In all other situations, the user is considered to be **offline**.

[scopeless-request]: https://user-images.githubusercontent.com/2988/38115271-65e65928-3379-11e8-9945-4a15ceb857fe.png

[user-scope-request]: https://user-images.githubusercontent.com/2988/38115272-65f69784-3379-11e8-8753-4da6547c7edb.png