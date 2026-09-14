.. meta::
   :description: Call flows route inbound calls to the appropriate users, groups, queues, menus,
                 voicemail, or external numbers. This page covers the configuration required, the
                 nodes available in the call flow builder, and the process of designing a flow.

.. |VOIP| replace:: :abbr:`VoIP (Voice over Internet Protocol)`

==========
Call flows
==========

A *call flow* determines what happens to an inbound call when it reaches an Odoo phone number: who
it rings, which menu it plays, and where it goes if nobody answers. The **Phone** app provides a
*call flow builder* to design call routes as a diagram of connected nodes. This article covers the
:ref:`configuration <phone/call_flows/configuration>` required to use call flows, the :ref:`nodes
<phone/call_flows/builder>` available in the *call flow builder*, and the :ref:`process
<phone/call_flows/design>` of designing a call flow.

.. _phone/call_flows/configuration:

Configuration
=============

Call flows require at least one phone number purchased through the Odoo Phone Service. To purchase a
number, go to :menuselection:`Phone app --> Phone Numbers --> Phone Numbers`.

The following resources can be attached to a call flow, and are managed under :menuselection:`Phone
--> Configuration`:

- :guilabel:`Extensions`: internal numbers used to reach a user directly.
- :guilabel:`Groups`: sets of users that ring together.
- :guilabel:`Queues`: agent queues that calls wait in until an agent is available.
- :guilabel:`Menus`: interactive voice menus (IVRs).
- :guilabel:`Voice Mailboxes`: voicemail boxes tied to a user or a queue.
- :guilabel:`Sounds` and :guilabel:`Music on Hold`: audio messages played to callers.
- :guilabel:`Time Conditions`: business-hours schedules used to route calls differently when open or
  closed.

.. tip::
   None of these resources need to be created in advance. Every node in the call flow builder that
   uses one of them can create it on the spot.

.. _phone/call_flows/builder:

Call flow builder
=================

To open the builder, go to :menuselection:`Phone --> Call Flows` and click :guilabel:`New`. Every
flow starts with a single :guilabel:`Start` node, shown as a green circular icon, representing the
moment a call comes in.

Add any of the following nodes from the sidebar on the left, either by dragging one onto the canvas
or by clicking it to append it to the last unconnected output:

- :guilabel:`Call a User`: ring a specific user, on their assigned extension or device.
- :guilabel:`Call a Contact`: ring a phone number stored on a contact record.
- :guilabel:`Call a Group`: ring every member of a call group at once.
- :guilabel:`Send to a Queue`: place the caller in an agent queue until an agent is available.
- :guilabel:`Open a Menu`: play an interactive voice menu (IVR) that routes the caller based on
  which key they press.
- :guilabel:`Play Audio`: play an uploaded or text-to-speech-generated audio message.
- :guilabel:`Send to Voicemail`: forward the caller directly to a voicemail box.
- :guilabel:`Redirect to an Extension`: transfer the call to an internal extension number.
- :guilabel:`Redirect to an External Number`: transfer the call to a number outside Odoo.
- :guilabel:`Time Condition`: route the call differently depending on whether it arrives during an
  :guilabel:`Open` or a :guilabel:`Closed` period.
- :guilabel:`Hang Up`: end the call. Shown as a red circular endpoint.

.. tip::
   Any node backed by a record, such as a queue, group, menu, contact, user, or audio message, opens
   a search window instead of a plain field. Use it to select an existing record, or click
   :guilabel:`New` to create one without leaving the flow.

.. _phone/call_flows/design:

Design a call flow
==================

#. Go to :menuselection:`Phone --> Call Flows`, click :guilabel:`New`, and name the flow.
#. Drag the small circle on the right of the :guilabel:`Start` node onto the first node to add, or
   click a node in the sidebar to append it automatically.
#. Configure the node. For record-backed nodes, search for an existing record, or click
   :guilabel:`New` to create one on the spot; the node is left unconfigured if the creation is
   cancelled.
#. Repeat for every branch, until each path ends in :guilabel:`Hang Up`, :guilabel:`Send to
   Voicemail`, or a redirect node.
#. Use the zoom controls (:guilabel:`+`, :guilabel:`-`, :guilabel:`Fit to content`) in the top-right
   corner of the canvas to navigate a large flow.
#. Save the flow.
#. Go to :menuselection:`Phone --> Phone Numbers`, open the number that should use this routing, and
   select the flow in its :guilabel:`Call Flow` field.

.. seealso::
   - :doc:`phone_widget`
   - :doc:`axivox/call_queues`

.. _phone/call_flows/troubleshooting:

Troubleshooting
===============

.. _phone/call_flows/troubleshooting-no-numbers:

No phone numbers to choose from
-------------------------------

Record pickers for nodes such as :guilabel:`Send to a Queue` show a :guilabel:`Buy a Number` button
instead of a list when no phone number has been purchased yet. Purchase a number under
:menuselection:`Phone --> Phone Numbers` first, then reopen the node.

.. _phone/call_flows/troubleshooting-time-condition-closed:

A Time Condition always takes the Closed branch
-----------------------------------------------

A :guilabel:`Time Condition` node warns when it has no :guilabel:`Open` period configured, since
every call then falls through to the :guilabel:`Closed` output regardless of when it arrives. Open
the node and add at least one open period to fix this.

.. _phone/call_flows/troubleshooting-old-members:

A queue or group node still shows old members
---------------------------------------------

Record-backed nodes refresh automatically after their configuration dialog is saved. If a node still
displays outdated members or agents, close and reopen the call flow to force a refresh.
