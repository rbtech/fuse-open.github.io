---
layout: example
title: LoadingIndicator
synopsis: Creating an animated loading indicator while using the BusyTask API.
date: 2016-12-28 00:00:00
uxConcepts:
- Cycle
- DockPanel
- Scale
- Spin
- Timeline
- ux:Class
- Busy
- WhileBusy
jsConcepts:
- Busy
preview-image: loading-indicator.png
preview-video: T8E_I8kD6Hc
---
This example shows how we can create an animated loading indicator. It includes a simulated background operation in JavaScript that triggers the visibility of our animation in the custom `MyLoadingIndicator` class. In this case we make use of `Busy` and `WhileBusy` triggers.

```
<App Background="#311B92">
	<iOS.StatusBarConfig Style="Light" />
	<JavaScript>
	exports.startLoading = function() {
		isBusy.activate();
		setTimeout(function() {
			isBusy.deactivate();
		}, 4000);
	};
	</JavaScript>

	<Panel ux:Class="MyLoadingIndicator" ThemeColor="#1565C0">
		<float4 ux:Property="ThemeColor" />
		<Circle ux:Name="rotatingStroke" Width="50" Height="50" StartAngleDegrees="-45" EndAngleDegrees="45">
			<Stroke Width="2" Color="{ReadProperty ThemeColor}" />
		</Circle>
		<Circle Color="{ReadProperty ThemeColor}" Width="20" Height="20">
			<Timeline ux:Name="myTimeline">
				<Scale Factor=".5" Duration=".25" Easing="CircularOut" EasingBack="CircularIn" />
				<Scale Factor="2" Duration=".25" Delay=".25" Easing="BounceOut" EasingBack="BounceIn" />
			</Timeline>
		</Circle>
		<WhileFalse>
			<Cycle Target="myTimeline.TargetProgress" Low="0" High="1" Frequency=".5" />
			<Spin Target="rotatingStroke" Frequency="1" />
		</WhileFalse>
	</Panel>

	<Panel Clicked="{startLoading}" HitTestMode="LocalBounds">
		<Busy ux:Name="isBusy" IsActive="false" />
		<WhileBusy>
			<Change loadingPanel.Opacity="1" Duration=".4" />
		</WhileBusy>
		<MyLoadingIndicator ux:Name="loadingPanel" Opacity="0" ThemeColor="#fff" />
		
		<Text Color="#fff6" Alignment="Bottom" TextAlignment="Center" Margin="0,40">Tap anywhere to load</Text>
	</Panel>
</App>
```
