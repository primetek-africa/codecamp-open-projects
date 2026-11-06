# MatatuTrack Commuter App

A route and arrival time app for minibus transit known as matatu in Kenya, danfo in Nigeria, trotro in Ghana, taxi brousse in francophone West Africa and combi in Southern Africa.

## Problem

Minibus transit moves most of urban Africa but has no schedule. Commuters guess when the next vehicle will arrive, operators have no ridership data, and transport authorities cannot plan without route level numbers.

MatatuTrack provides a route catalogue, crowdsourced arrival estimates and an operator dashboard with ridership and fare data.

## Target users

Primary users are urban commuters. Secondary users are matatu or danfo SACCO operators and conductors. Tertiary users are city transport authorities and researchers.

## MVP features

1. Route catalogue. Named routes, stops and typical fares, editable by moderators.
2. Vehicle crowdsourced position. Conductors or commuters report that a vehicle is at a stop. A simple arrival estimate per route.
3. Conductor fare entry. Conductor records a fare for each passenger. Aggregated per shift.
4. Operator dashboard. Daily ridership and fare summary per route.
5. SACCO admin view. Fleet level numbers.
6. Simple API for city researchers.

## Post MVP ideas

GPS tracker integration via a cheap Bluetooth or dongle device for premium operators, live boarding and alighting map, prepaid pass via mobile money, fare optimization suggestions, authority exports.

## User stories

As a commuter I want to know roughly when the next matatu will reach my stop. As a conductor I want a one tap fare entry. As a SACCO manager I want to know how much route 46 made yesterday.

## Success metrics

Routes mapped, daily active commuters, conductors active, SACCOs reporting, authority downloads.

## Revenue model

Operator subscription, in app ads on route cards, authority data licensing.

## Risks and compliance

Do not track individual commuters by default. Vehicle level position is enough. Keep conductor fare data private to the operator. Clearly disclose that arrival estimates are crowdsourced.

## Research references

GTFS and GTFS Realtime, Digital Matatus project at the University of Nairobi, Trufi Association open data, WhereIsMyTransport research.
